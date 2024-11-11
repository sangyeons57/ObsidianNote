
```Java
public class EditCategoryDialogFragment extends DialogFragment {  
    public static final String CATEGORY_ID = "categoryId";  
    public static final String CATEGORY_NAME = "categoryName";  
  
    public static EditCategoryDialogFragment Instance(int categoryId, String categoryName){  
        EditCategoryDialogFragment dialogFragment = new EditCategoryDialogFragment();  
        Bundle bundle = new Bundle();  
        bundle.putInt(CATEGORY_ID, categoryId);  
        bundle.putString(CATEGORY_NAME, categoryName);  
        dialogFragment.setArguments(bundle);  
        return  dialogFragment;  
    }
```

이런식으로 Instance를 만들어서
Fragment에 대한 속성등의 설정을 다 한후에

```Java
EditCategoryDialogFragment.Instance(item.categoryId, item.name)  
        .show(fragmentManager, "EditCategoryDialogFragment");
```
이런 식으로 사용해서 쓸수있다.
