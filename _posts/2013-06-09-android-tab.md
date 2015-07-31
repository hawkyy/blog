---
layout: post
title:  "Android使用tabhost实现类似微信tab界面"
date:   2013-06-09 14:06:05
categories: android
---

* content
{:toc}

activity_bottom_tab.xml

    <?xml version="1.0" encoding="utf-8"?>
    <TabHost xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent" android:layout_height="match_parent"
        android:id="@android:id/tabhost"  >
        <LinearLayout
            android:layout_width="fill_parent"
            android:layout_height="fill_parent"
            android:orientation="vertical">
            <FrameLayout
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:id="@android:id/tabcontent"
                android:layout_weight="1.0"
                android:layout_gravity="center" />
            <View
                android:layout_width="fill_parent"
                android:layout_height="1dp"
                android:background="@android:color/darker_gray"/>
            <TabWidget
                android:layout_width="fill_parent"
                android:layout_height="wrap_content"
                android:id="@android:id/tabs"
                android:layout_weight="0.0"  />
        </LinearLayout>
    </TabHost>
    
activity实现

    public class BottomTabActivity extends TabActivity implements TabHost.OnTabChangeListener {
        private List<LinearLayout> viewsList;
        private int[] normalResId = { R.drawable.device_normal,
                R.drawable.template_normal,
                R.drawable.about_normal };
        private int[] selectResId = { R.drawable.device_selected,
                R.drawable.template_selected,
                R.drawable.about_selected };

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_bottom_tab);
            viewsList = new ArrayList<LinearLayout>();
            addTabs();
        }

        protected void addTabs() {
            TabHost tabHost;
            tabHost = getTabHost();
            tabHost.addTab(tabHost.newTabSpec(getString(R.string.tab_device))
                    .setIndicator(composeLayout(getString(R.string.tab_device), R.drawable.device_normal))
                    .setContent(new Intent(this, DeviceListActivity.class)));
            tabHost.addTab(tabHost.newTabSpec(getString(R.string.tab_template))
                    .setIndicator(composeLayout(getString(R.string.tab_template), R.drawable.template_normal))
                    .setContent(new Intent(this, TemplateListActivity.class)));
            tabHost.addTab(tabHost.newTabSpec(getString(R.string.tab_about))
                    .setIndicator(composeLayout(getString(R.string.tab_about), R.drawable.about_normal))
                    .setContent(new Intent(this, AboutActivity.class)));
            tabHost.setCurrentTab(0);
            changeBottomState(0);
            tabHost.setOnTabChangedListener(this);
        }
    
        private View composeLayout(String s, int i) {
            LinearLayout layout = new LinearLayout(this);
            layout.setOrientation(LinearLayout.VERTICAL);
            ImageView iv = new ImageView(this);
            iv.setImageResource(i);
            layout.addView(iv, new LinearLayout.LayoutParams(LinearLayout.LayoutParams.FILL_PARENT,
                    LinearLayout.LayoutParams.WRAP_CONTENT));
            TextView tv = new TextView(this);
            tv.setGravity(Gravity.CENTER);
            tv.setSingleLine(true);
            tv.setText(s);
            tv.setTextColor(Color.GRAY);
            tv.setTextSize(12);
            layout.addView(tv, new LinearLayout.LayoutParams(LinearLayout.LayoutParams.FILL_PARENT,
                    LinearLayout.LayoutParams.WRAP_CONTENT));
            layout.setGravity(Gravity.CENTER_VERTICAL);
            viewsList.add(layout);
            return layout;
        }
    
        @Override
        public void onTabChanged(String tabId) {
            if (getString(R.string.tab_device).equals(tabId)) {
                changeBottomState(0);
            } else if (getString(R.string.tab_template).equals(tabId)) {
                changeBottomState(1);
            } else if (getString(R.string.tab_about).equals(tabId)) {
                changeBottomState(2);
            }
        }
    
        private void changeBottomState(int index) {
            LinearLayout layout;
            ImageView iv;
            TextView tv;
            for (int i = 0; i < viewsList.size(); i++) {
                layout = viewsList.get(i);
                iv = (ImageView) layout.getChildAt(0);
                tv = (TextView) layout.getChildAt(1);
                if (index == i) {
                    tv.setTextColor(getResources().getColor(R.color.tab_green));
                    iv.setImageResource(selectResId[i]);
                } else {
                    layout.setBackgroundResource(0);
                    tv.setTextColor(getResources().getColor(R.color.tab_black));
                    iv.setImageResource(normalResId[i]);
                }
            }
        }
    }
