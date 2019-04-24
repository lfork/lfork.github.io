# 1. 常用命令
获取当前activity包名【Windows】
adb shell dumpsys window windows | findstr "mFocusedApp" 

获取当前activity包名  
dumpsys window windows | grep -E 'mCurrentFocus|mFocusedApp' 
pm disable com.tencent.mm/.plugin.webview.ui.tools.WebViewUI 禁用Activity 注意/斜杠


# 2. 手机限制 pm disable /enable

## 2.1 手机QQ
+ 浏览器
com.tencent.mobileqq/com.tencent.mobileqq.activity.QQBrowserActivity

+ 其他
com.tencent.mobileqq/.activity.LebaListMgrActivity
com.tencent.mobileqq/.activity.NearbyActivity
com.tencent.mobileqq/com.tencent.biz.pubaccount.readinjoy.view.fastweb.FastWebActivity
com.tencent.mobileqq/.activity.AssistantSettingActivity t17832
com.tencent.mobileqq/com.tencent.biz.pubaccount.readinjoy.activity.ReadInJoyNewFeedsActivity t17832
com.tencent.mobileqq/com.tencent.biz.pubaccount.PublicAccountBrowser
## 2.2 禁止APP的安装

com.android.packageinstaller/.PackageInstallerActivity t17833




