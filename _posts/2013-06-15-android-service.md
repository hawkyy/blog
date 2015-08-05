---
layout: post
title:  "Android service"
date:   2013-06-09 14:06:05
categories: Android
---

* content
{:toc}

在AndroidManifest.xml中

    <manifest>
        <application>
        ......
            <service android:name="com.mycorp.ikit.service.MessageService" android:process=":remote">
                <intent-filter>
                    <action android:name="com.mycorp.ikit.service.MessageService"></action>
                </intent-filter>
            </service>
        </application>
    </manifest>

AIDL定义 IMessageService.aidl

    package com.mycorp.ikit.service;
    interface IMessageService {
        int getHttpPort();
        int getCtrlPort();
        void stop();
    }

MessageBinder.java

    public class MessageBinder extends IMessageService.Stub {
        private IoTNative iot = new IoTNative();

        public MessageBinder(String iotDir, String imei, String sversion) {
            Log.i("ikit binder", "in MessageBinder");
            if (!IoTNative.LoadLib())
            {
                Log.i("ikit binder", "Load iotback lib fail");
                return;
            }

            iot = new IoTNative();
            iot.init(iotDir, Locale.getDefault().getLanguage(), "android", Build.VERSION.RELEASE, Build.DEVICE, imei);
        }

        @Override
        public int getHttpPort() throws RemoteException {
            return iot.getHttpPort();
        }

        @Override
        public int getCtrlPort() throws RemoteException {
            return iot.getCtrlPort();
        }

        @Override
        public void stop() throws RemoteException {
            iot.stop();
        }
    }
    
MessageService.java

    public class MessageService extends Service {
        private MessageBinder mBinder;
    
        	@Override
        	public IBinder onBind(Intent intent) {
        		// TODO Auto-generated method stub
            Log.i("service", "onBind");
        		return mBinder;
        	}

        @Override
        public void onCreate() {
            String sversion = "";
            super.onCreate();

            ......
            mBinder = new MessageBinder(getDir("iot", 0).getPath(), imei, sversion);
            ......
        }
    }

在Activity或者Application中启动Service

    public class MainApp extends Application {
        IMessageService mMessageService;

        private ServiceConnection mConnection = new ServiceConnection() {
            @Override
            public void onServiceConnected(ComponentName name, IBinder service) {
                mMessageService = IMessageService.Stub.asInterface(service);
                Log.i("ikit", "onServiceConnected");
            }

            @Override
            public void onServiceDisconnected(ComponentName name) {
                mMessageService = null;
            }
        };

        @Override
        public void onCreate() {
            super.onCreate();
    
            startService(new Intent("com.mycorp.ikit.service.MessageService"));
            bindService(new Intent("com.mycorp.ikit.service.MessageService"), mConnection, BIND_AUTO_CREATE);
        }

        @Override
        public void onTerminate() {
            if (mConnection != null)
            {
                unbindService(mConnection);
                mConnection = null;
            }
            //stopService(new Intent(this, MessageService.class));
            super.onTerminate();
        }

        public IMessageService getMessageService() {
            return mMessageService;
        }
        ......
    }
