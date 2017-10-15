# Fragment1
- 메인 액티비티에서 버튼 2개있음
- 1. 메인프래그먼트 2. 메뉴프래그먼트
- 각 버튼 클릭하면 해당 프래그먼트가 나옴.

## 주요메소드 및 사용법

### MainActivity

```Java
public class MainActivity extends AppCompatActivity {
    MainFragment fragment1;
    MenuFragment fragment2;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        fragment1 = new MainFragment();
        fragment2 = new MenuFragment();


        Button button = (Button) findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {

                getSupportFragmentManager().beginTransaction().replace(R.id.container, fragment1).commit();// 트랜젝션에 들어가야하므로 commit!
            }
        });

        Button button2 = (Button) findViewById(R.id.button2);
        button2.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {

                getSupportFragmentManager().beginTransaction().replace(R.id.container, fragment2).commit();// 트랜젝션에 들어가야하므로 commit!
            }
        });
    }

    public void onFragmentChange(int index){
        if(index ==0){
            getSupportFragmentManager().beginTransaction().replace(R.id.container, fragment1).commit();
        }else if (index ==1){
            getSupportFragmentManager().beginTransaction().replace(R.id.container, fragment2).commit();
        }
    }
}
```

- 메인액티비티 내에 두개의 프래그먼트 객체 생성
- 버튼을 눌렀을때 프래그먼트 나옴
- replace함수를 사용해 프래그먼트간 이동할 수 있음
- index값을 주어 두 프래그먼트간 구별

### MainFragment

```Java
public class MainFragment extends Fragment {

    MainActivity activity;

    @Override
    public void onAttach(Context context) {
        super.onAttach(context);

        activity = (MainActivity) getActivity();
    }

    @Override
    public void onDetach() {
        super.onDetach();
        activity = null;
    }

    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        ViewGroup rootView = (ViewGroup) inflater.inflate(R.layout.fragment_main, container, false);


        Button button = (Button)rootView.findViewById(R.id.button2);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {

                activity.onFragmentChange(1);
            }
        });
        return rootView;
    }
}
```

- 해당 레이아웃에 인플레이트해줌.(프래그먼트는 액티비티가 아니므로 인플레이트를 통해 연결해주어야 함.)
- onAttach함수가 불릴때(액티비티와 프래그먼트가 붙을때) 액티비티를 가지고 와서 액티비티 내에 있는 onFragmentChange메서드를 활용해 인덱스 값을 바꿔줌

### MenuFragment

```Java
public class MenuFragment extends Fragment {


    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
       ViewGroup rootView = (ViewGroup) inflater.inflate(R.layout.fragment_menu, container, false);

        return rootView;
    }
}
```

- 기본값은 0이니 따로 리스너를 안 달아줌?
- 생각 필요함
