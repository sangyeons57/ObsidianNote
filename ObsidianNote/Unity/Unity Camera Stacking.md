#unity/Camera
Unity Camera Stacking 이란
[영상](https://youtu.be/OmCjPctKkjw?si=7HRBpEhnRvaptPgf)을 봐라 , 카메라 2개 이상을 겹처서 쓰는거다

built-in pipeline에서는 depth only 를 통해서 카매라를에 비추는것을 합칠수가잇었는데
urp에서는 불가능 해져서 해당 방법이 생긴거같다.

buit-in에서 하는 방법 [영상](https://youtu.be/bbnVpPiQ_rU?si=WwpgP23Qr1hLsSsp)

내가 쓴 방법은 ui용 카메라와 게임용 카메라를 나눠서 쓰는 것이다.
