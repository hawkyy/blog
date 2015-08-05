---
layout: post
title:  "Android Broadcast"
date:   2013-06-09 14:06:05
categories: Android
---

* content
{:toc}

注册接收端

        BroadcastReceiver mReceiver;
        LocalBroadcastManager mLocalBroadcastManager;
    
        IntentFilter filter = new IntentFilter();
        filter.addAction("com.my.app");
        mReceiver = new BroadcastReceiver() {
            @Override
            public void onReceive(Context context, Intent intent) {
                if (intent.getAction().equals("com.my.app")) {
                    String msg = intent.getStringExtra("msg");
                }
            }
        };
        mLocalBroadcastManager = LocalBroadcastManager.getInstance(this);
        mLocalBroadcastManager.registerReceiver(mReceiver, filter);

反注册

        mLocalBroadcastManager.unregisterReceiver(mReceiver);

发送广播

        LocalBroadcastManager lbm = LocalBroadcastManager.getInstance(mApplication);
        Intent intent = new Intent("com.my.app");
        intent.putExtra("msg", msg);
        lbm.sendBroadcast(intent);
