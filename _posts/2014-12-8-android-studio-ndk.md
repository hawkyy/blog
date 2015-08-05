---
layout: post
title:  "Android Studio工程中ndk项目处理"
date:   2014-12-8 14:06:05
categories: Android
---

* content
{:toc}

Android Studio对NDK的支持一直不完善，可以直接使用建立Makefile编译

    ANDROID_DIR := $(shell pwd)
    TOP_DIR := $(shell pwd)/../
    PHONEBACK := $(TOP_DIR)/phoneback/
    ASSETS_DIR := $(ANDROID_DIR)/app/src/main/assets/
    JNILINK := app/src/main/jni

    all:
        	rm -f $(ANDROID_DIR)/$(JNILINK)/; ln -s -f $(PHONEBACK) $(ANDROID_DIR)/$(JNILINK); cd $(PHONEBACK); ndk-build NDK_PROJECT_PATH=$(ANDROID_DIR)/app/src/main; rm $(ANDROID_DIR)/$(JNILINK)
        	rm $(ASSETS_DIR)/web/ -rf
        	cp $(PHONEBACK)/web/ $(ASSETS_DIR) -rf
        	./gradlew build

    clean:
        	rm -f $(ANDROID_DIR)/$(JNILINK)/; ln -s -f $(PHONEBACK) $(ANDROID_DIR)/$(JNILINK); cd $(PHONEBACK); ndk-build          NDK_PROJECT_PATH=$(ANDROID_DIR)/app/src/main clean; rm $(ANDROID_DIR)/$(JNILINK)
    	./gradlew clean
