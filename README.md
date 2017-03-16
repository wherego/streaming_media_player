用[这里](https://github.com/upyun/android-player-sdk)的sdk实现转播斗鱼直播平台的直播视频。
## 1.android-player-sdk-master
原工程文件
## 2.import_demo
导入的demo
## 3.Material-BottomNavigation-master
[这里](https://github.com/sephiroth74/Material-BottomNavigation)的底部导航菜单demo
## 抓取斗鱼直播源的方法,斗鱼不定时会变,来自[这里](https://github.com/littleMeng/video-live)
==== 2017.3.16测试有效 ====
```
public void getStreamUrl(final int roomId) {
  //get请求地址
  String url = "http://coapi.douyucdn.cn/lapi/live/thirdPart/getPlay/"+roomId+"?rate=0";

  //volley
  StringRequest request = new StringRequest(Request.Method.GET, url,new Response.Listener<String>() {
    @Override
    public void onResponse(String response) {
      Log.i(TAG, response);
    }
  }, new Response.ErrorListener() {
    @Override
    public void onErrorResponse(VolleyError error) {

    }
  }) {
    @Override
    public Map<String, String> getHeaders() throws AuthFailureError {
      return getDouyuRoomParams(roomId);
    }
  };

  //将request加入请求队列
  mRequestQueue.add(request);

  //查看信息
  try {
    System.out.println("请求头："+request.getHeaders());
    System.out.println("请求BodyContentType："+request.getBodyContentType());
    System.out.println("请求CacheKey："+request.getCacheKey());
    System.out.println("请求Url："+request.getUrl());
    System.out.println("请求Method："+request.getMethod());
    System.out.println("请求Tag："+request.getTag());
  }catch(Exception e){

  }

}

/*
* 请求头
*/
public HashMap<String, String> getDouyuRoomParams(int roomId) {
  int time = (int) (System.currentTimeMillis()/1000);
  String signContent = "lapi/live/thirdPart/getPlay/" + roomId + "?aid=pcclient&rate=0&time=" + time + "9TUk5fjjUjg9qIMH3sdnh";
  String sign = strToMd5Low32(signContent);

  HashMap<String, String> map = new HashMap<>();
  map.put("auth", sign);
  Log.i(TAG,"auth:"+sign);
  map.put("time", Integer.toString(time));
  Log.i(TAG,"time:"+Integer.toString(time));
  map.put("aid", "pcclient");

  return map;
}

/*
* 加密str
*/
public static String strToMd5Low32(String str) {
  StringBuilder builder = new StringBuilder();
  try {
    MessageDigest md5 = MessageDigest.getInstance("MD5");
    md5.update(str.getBytes());
    byte[] bytes = md5.digest();
    for (byte b : bytes) {
      int digital = b&0xff;
      if (digital < 16)
        builder.append(0);
      builder.append(Integer.toHexString(digital));
    }
  } catch (NoSuchAlgorithmException e) {
    e.printStackTrace();
  }
  return builder.toString().toLowerCase();
}

```