메세지가 동시에 전송될떄 메세지가 분리되서 인식되어야하는경우 (JSON형식) 
데이터를 를 항상 고정되 만큼 읽는 방식으로 할경우 문제가 발생함

데이터를 클라이언트에서 보네도 즉시 서버가 받는게 아니라
일정시간 도안 버퍼에 모아두었다가 읽게 되기때문에 서로 나눠져서 읽어야할 데이터가 합쳐저이있어서 오류가 발생하는데

위에 해당 문제가 데이터를 읽은 다음 에 날려서 요청한 데이터가 다 없어진다는 문제가 생기기도 했음 

해당 문제를 해결하기위해서는 여러가지 방법이 있지만 해당 방법이 가장 적절하다고 생각해서 이방식으로 구현했음
명시적으로 데이터의 크기를 알려줘서 읽게하는 방식을 사용함
해당 방식은 데이터의 길이를 먼져 알리고 해당 데이터 만큼만 읽게 한뒤 데이터를 날림 (flip)

그후 버퍼에서 추가로  길이 데이터를 받아서 위 코드를 반복함


서버 코드
```JAVA
public static class ChannelBuffer{  
    public static final int LENGTH_FIELD_SIZE = 4;  
    public ByteBuffer lengthBuffer = ByteBuffer.allocate(LENGTH_FIELD_SIZE);  
    public ByteBuffer messageBuffer = null;  
    public int expectedLength = -1;  
}

public void Start(){  
    try {  
        Selector selector = Selector.open();  
        ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();  
        serverSocketChannel.bind(new InetSocketAddress(PORT));  
        serverSocketChannel.configureBlocking(false);  
        serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);  
  
        System.out.println("version: 0.1");  
        System.out.println("Chat server started on port " + PORT);  
  
        while (true) {  
            selector.select();  
            Set<SelectionKey> selectedKeys = selector.selectedKeys();  
            Iterator<SelectionKey> keyIterator = selectedKeys.iterator();  
  
            while (keyIterator.hasNext()) {  
                SelectionKey key = keyIterator.next();  
  
                try {  
                    if (key.isAcceptable()) {  
                        handleAccept(serverSocketChannel, selector);  
                    } else if (key.isReadable()) {  
                        handleRead(key);  
                    }  
                } catch (IOException e) {  
                    key.cancel();  
                    try {  
                        key.channel().close();  
                    } catch (IOException ex){  
                        ex.printStackTrace();  
                    }  
                    System.out.println("Client disconnected unexpectedly");  
                    e.printStackTrace();  
                    System.out.println(e.getMessage());  
                }  
  
                keyIterator.remove();  
            }  
        }  
    } catch (IOException e) {  
        e.printStackTrace();  
    }  
}

private void handleAccept(ServerSocketChannel serverSocketChannel, Selector selector) throws IOException {  
    SocketChannel clientChannel = serverSocketChannel.accept();  
    clientChannel.configureBlocking(false);  
    clientChannel.register(selector, SelectionKey.OP_READ);  
    System.out.println("New client connected: " + clientChannel.getRemoteAddress());  
}  
  
private void handleRead(SelectionKey key) throws IOException {  
    SocketChannel clientChannel = (SocketChannel) key.channel();  
    ChannelBuffer buffer = channelBufferMap.computeIfAbsent(clientChannel, k -> new ChannelBuffer());  
  
    try {  
        if (buffer.messageBuffer == null) {  
            readMessageLength(clientChannel, buffer);  
        }  
  
        if(buffer.messageBuffer != null){  
            readMessageContent(clientChannel, buffer);  
        }  
    } catch (IOException e) {  
        clientChannel.close();  
        System.out.println("Client disconnected");  
    } catch (JSONException e) {  
        trySendMessage(clientChannel, new JSONObject()  
                .put(SocketEventListener.eKey.TYPE.toString(), SocketEventListener.eType.ERROR.toString())  
                .put(SocketEventListener.eKey.MESSAGE.toString(), e.getMessage()));  
    }  
}

public void readMessageLength(SocketChannel clientChannel, ChannelBuffer buffer) throws IOException {  
    clientChannel.read(buffer.lengthBuffer);  
    if(!buffer.lengthBuffer.hasRemaining()) {  
        buffer.lengthBuffer.flip();  
        buffer.expectedLength = buffer.lengthBuffer.getInt();  
        buffer.messageBuffer = ByteBuffer.allocate(buffer.expectedLength);  
        buffer.lengthBuffer.clear();  
    }  
}  
  
public void readMessageContent(SocketChannel clientChannel, ChannelBuffer buffer) throws IOException {  
    clientChannel.read(buffer.messageBuffer);  
    if(!buffer.messageBuffer.hasRemaining()){  
        buffer.messageBuffer.flip();  
        String jsonString = StandardCharsets.UTF_8.decode(buffer.messageBuffer).toString();  
        System.out.println("RECEIVED [" + channelMap.get(clientChannel) + "]: " + jsonString);  
        SocketEventListener.callEvent(clientChannel, new JSONObject(jsonString));  
        buffer.messageBuffer = null;  
        buffer.expectedLength = -1;  
    }  
}
```

클라이언트 코드
```JAVA
public Thread networkThread;  
  
public BlockingQueue<String> taskQueue ;  
public List<SocketEventListener.eType> waitingListenList;  
  
private BufferedReader in;  
private DataOutputStream out;  
  
private SocketConnection(){  
    taskQueue = new LinkedBlockingQueue<>();  
    waitingListenList = new ArrayList<>();  
  
    networkThread = new Thread(new Runnable() {  
        @Override  
        public void run() {  
            try {  
                Socket clientSocket = new Socket(SERVER_ADDRESS, PORT);  
  
                in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));  
                out = new DataOutputStream(clientSocket.getOutputStream());  
  
                LOG(out.toString());  
                new Thread(new Runnable() {  
                    @Override  
                    public void run() {  
                        try {  
                            String message;  
                            while ((message = in.readLine()) != null) {  
                                callEvent(message);  
                            }  
                        } catch (IOException e) {  
                            e.printStackTrace();  
                            throw new RuntimeException(e);  
                        }  
                    }  
                }).start();  
                new Thread(new Runnable() {  
                    @Override  
                    public void run() {  
                        try {  
                            while (true){  
                                String message = taskQueue.take();  
                                byte[] jsonBytes = message.getBytes(StandardCharsets.UTF_8);  
                                out.writeInt(jsonBytes.length);  
                                out.write(jsonBytes);  
                                out.flush();  
                                LOG("FINISH Sent to Server", message);  
                            }  
                        } catch (InterruptedException e) {  
                            Thread.currentThread().interrupt();  
                            e.printStackTrace();  
                        } catch (IOException e) {  
                            throw new RuntimeException(e);  
                        }  
                    }  
                }).start();  
            } catch (UnknownHostException e) {  
                e.printStackTrace();  
                LOGe(e.getMessage());  
                /**  
                 * 오프라인 기능 대처하는 만들려면 이쪽에서 구현하면됨  
                 */  
                throw new RuntimeException(e);  
            } catch (IOException e) {  
                e.printStackTrace();  
                LOGe(e.getMessage());  
                throw new RuntimeException(e);  
            }  
        }  
    });  
    networkThread.start();  
}
```

여기서 사용한 방식은
- 클라이언트에서 메세지를 보넬때 먼저 4byte에 전체JSON 데이터의 길이를 포함해서 보내서 먼저 읽게 한다음 추가로 해당 길이 만큼 매세지를 읽어서 데이터를 가져오는 방식이다.

한번에 다 메세지를 못보네는 경우를 고려해서
메세지 길이를 다 세지 못했을면 버퍼를 null로해서 메인 메시지를 못 일게하고

메세지의 길이를 알았으면 버퍼를 생성해서
해당 버퍼로 데이터를 읽고 다시 버퍼를 빈상태로 만든다.

또한 각 연결마다. ChannelBuffer를 따로 만들어서 여러며의 동시접속에 대비한다.

