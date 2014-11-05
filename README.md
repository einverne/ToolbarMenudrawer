ToolbarMenudrawer Demo
=================

**Documentation**

###Custom TextView

    public class ToolbarMenudrawerTextView extends TextView {
    
        public ToolbarMenudrawerTextView(Context context, AttributeSet attrs,
                                         int defStyle) {
            super(context, attrs, defStyle);
            init(attrs);
        }
    
        public ToolbarMenudrawerTextView(Context context, AttributeSet attrs) {
            super(context, attrs);
            init(attrs);
    
        }
    
        public ToolbarMenudrawerTextView(Context context) {
            super(context);
            init(null);
        }
    
        private void init(AttributeSet attrs) {
            if (attrs != null) {
                TypedArray a = getContext().obtainStyledAttributes(attrs,
                        R.styleable.ToolbarMenuDrawer);
                String fontName = a
                        .getString(R.styleable.ToolbarMenuDrawer_fontName);
                if (fontName != null) {
                    if (!isInEditMode()){
                        Typeface myTypeface = Typeface.createFromAsset(getContext()
                                .getAssets(), "fonts/" + fontName);
                        setTypeface(myTypeface);
                    }
                }
                a.recycle();
            }
        }
    
    }
set Font on these Text

###Usage of DrawerLayout
	android.support.v4.widget.DrawerLayout
	
android:layout_gravity 属性决定从左右滑出
一般情况高度match_parent，宽度自适应
DrawerLayout.DrawerListener 监听drawer状态

将这个Drawer分成Head，ListView，Foot，三部分可以分别进行设计，然后利用ListView的`addHeaderView`,`addFooterView`将头部和尾部加入。

    

    //get Toolbar
    mToolbar = (Toolbar) findViewById(R.id.toolbar);
    setSupportActionBar(mToolbar);
    getSupportActionBar().setTitle(R.string.blank_text);
    mTitle = mDrawerTitle = getTitle();
    mDrawerLayout = (DrawerLayout) findViewById(R.id.drawer_layout);
    mDrawerList = (ListView) findViewById(R.id.left_drawer);
    mDrawerLayout.setDrawerShadow(R.drawable.drawer_shadow,
            GravityCompat.START);
    //get navigation string Array
    MDTitles = getResources().getStringArray(
            R.array.navigation_main_sections);
    //get navigation drawables
    MDIcons = getResources().obtainTypedArray(R.array.drawable_ids);

    icons = new ArrayList<Icons>();
    icons.add(new Icons(MDTitles[0], MDIcons.getResourceId(0, -1)));
    icons.add(new Icons(MDTitles[1], MDIcons.getResourceId(1, -2)));
    icons.add(new Icons(MDTitles[2], MDIcons.getResourceId(2, -3)));

    //don't forget
    MDIcons.recycle();

    //adapter for listview in navigationbar
    adapter = new ToolbarMenudrawerAdapter(getApplicationContext(), icons);
    mDrawerList.setAdapter(adapter);

    //set listview listener
    mDrawerList.setOnItemClickListener(new DrawerItemClickListener());

    LayoutInflater inflater = getLayoutInflater();
    final ViewGroup header = (ViewGroup) inflater.inflate(R.layout.header,
            mDrawerList, false);            //navigation drawer header

    final ViewGroup footer = (ViewGroup) inflater.inflate(R.layout.footer,
            mDrawerList, false);

    mDrawerList.addHeaderView(header, null, true); // true = clickable
    mDrawerList.addFooterView(footer, null, true); // true = clickable

    mDrawerLayout = (DrawerLayout) findViewById(R.id.drawer_layout);
    mDrawerToggle = new ActionBarDrawerToggle(
            this,
            mDrawerLayout,
            mToolbar,
            R.string.drawer_open,
            R.string.drawer_close) {

        public void onDrawerClosed(View view) {
            invalidateOptionsMenu();
        }

        public void onDrawerOpened(View drawerView) {
            invalidateOptionsMenu();
        }

        @Override
        public void onDrawerSlide(View drawerView, float slideOffset) {
            /* TODO:
            Add stuff. :p */
        }
    };
    mDrawerToggle.syncState();
    mDrawerLayout.setDrawerListener(mDrawerToggle);

http://developer.android.com/reference/android/support/v4/widget/DrawerLayout.html

