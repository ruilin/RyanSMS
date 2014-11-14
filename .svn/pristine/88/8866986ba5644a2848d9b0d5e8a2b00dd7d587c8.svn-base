package com.ryan.sms;

import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.database.Cursor;
import android.net.Uri;
import android.os.Bundle;
import android.os.Message;
import android.provider.ContactsContract;
import android.provider.ContactsContract.CommonDataKinds.Phone;
import android.telephony.SmsMessage;

import com.ryan.util.SysUtil;

public class SmsReceiver extends BroadcastReceiver {

    public static final String SMS_RECEIVED_ACTION = "android.provider.Telephony.SMS_RECEIVED";
    public static final String SMS_KEY_ADDRESS = "address";
    public static final String SMS_KEY_NAME = "name";
    public static final String SMS_KEY_CONTENT = "content";
    public static final String SMS_KEY_TIME = "time";
    
    
	public SmsReceiver() {
	}

	@Override
	public void onReceive(Context context, Intent intent) {
		 if (intent.getAction().equals(SMS_RECEIVED_ACTION)) {
			 SmsMessage[] messages = getMessagesFromIntent(intent);
			 for (SmsMessage message : messages) {
				Bundle bundle = new Bundle();
				String address = message.getDisplayOriginatingAddress();
				String name = getPeopleNameFromPerson(context, address);
				bundle.putString(SMS_KEY_ADDRESS, address);
				bundle.putString(SMS_KEY_NAME, name);
				bundle.putString(SMS_KEY_CONTENT, message.getDisplayMessageBody());
				bundle.putString(SMS_KEY_TIME, SysUtil.long2Date(message.getTimestampMillis()));
				
				if (null != SmsMain.mInstance) {
					Message msg = new Message();
					msg.what = 0;
					msg.obj = bundle;
					SmsMain.mInstance.handler.sendMessage(msg);
				} else {
					Intent activityIntent = new Intent(context, SmsMain.class);
					activityIntent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);  
					activityIntent.putExtras(bundle);
					context.startActivity(activityIntent);
				}
			 }
		 }
	}
	
    public final SmsMessage[] getMessagesFromIntent(Intent intent) {
        Object[] messages = (Object[]) intent.getSerializableExtra("pdus");
        byte[][] pduObjs = new byte[messages.length][];
        for (int i = 0; i < messages.length; i++) {
            pduObjs[i] = (byte[]) messages[i];
        }
        byte[][] pdus = new byte[pduObjs.length][];
        int pduCount = pdus.length;
        SmsMessage[] msgs = new SmsMessage[pduCount];
        for (int i = 0; i < pduCount; i++) {
            pdus[i] = pduObjs[i];
            msgs[i] = SmsMessage.createFromPdu(pdus[i]);
        }
        return msgs;
    }
    
	
    // 通过address手机号关联Contacts联系人的显示名字  
	private static String getPeopleNameFromPerson(Context ctx, String address) {
		if(address == null || address.equals("")){
			return "";  
		}
		
		String strPerson = address;  
		String[] projection = new String[] {Phone.DISPLAY_NAME, Phone.NUMBER};  
		
		Uri uri_Person = Uri.withAppendedPath(ContactsContract.CommonDataKinds.Phone.CONTENT_FILTER_URI, address);  // address 手机号过滤  
		Cursor cursor = ctx.getContentResolver().query(uri_Person, projection, null, null, null);  
		
		if (cursor.moveToFirst()) {  
			int index_PeopleName = cursor.getColumnIndex(Phone.DISPLAY_NAME);  
			String strPeopleName = cursor.getString(index_PeopleName);  
			strPerson = strPeopleName;  
		}  
		cursor.close();  
		
		return strPerson;  
	}  

}
