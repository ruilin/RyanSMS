package com.ryan.util;

import android.annotation.SuppressLint;
import java.text.SimpleDateFormat;
import java.util.Date;

public class SysUtil {

	public SysUtil() {
		// TODO Auto-generated constructor stub
	}
	
	@SuppressLint("SimpleDateFormat")
	public static String long2Date(long ms) {
		SimpleDateFormat sdf= new SimpleDateFormat("yyyy-MM-dd HH:mm a");
		java.util.Date dt = new Date(ms); 
		String sDateTime = sdf.format(dt);  //得到精确到秒的表示：08/31/2006 21:08:00
		return sDateTime;
	}
}
