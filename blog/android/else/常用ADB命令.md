# 1. ��������
��ȡ��ǰactivity������Windows��
adb shell dumpsys window windows | findstr "mFocusedApp" 

��ȡ��ǰactivity����  
dumpsys window windows | grep -E 'mCurrentFocus|mFocusedApp' 
pm disable com.tencent.mm/.plugin.webview.ui.tools.WebViewUI ����Activity ע��/б��


# 2. �ֻ����� pm disable /enable

## 2.1 �ֻ�QQ
+ �����
com.tencent.mobileqq/com.tencent.mobileqq.activity.QQBrowserActivity

+ ����
com.tencent.mobileqq/.activity.LebaListMgrActivity
com.tencent.mobileqq/.activity.NearbyActivity
com.tencent.mobileqq/com.tencent.biz.pubaccount.readinjoy.view.fastweb.FastWebActivity
com.tencent.mobileqq/.activity.AssistantSettingActivity t17832
com.tencent.mobileqq/com.tencent.biz.pubaccount.readinjoy.activity.ReadInJoyNewFeedsActivity t17832
com.tencent.mobileqq/com.tencent.biz.pubaccount.PublicAccountBrowser
## 2.2 ��ֹAPP�İ�װ

com.android.packageinstaller/.PackageInstallerActivity t17833




