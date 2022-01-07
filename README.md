# sniffWebkit
基于AgentWeb的视频嗅探器
# sniffWebkit
基于AgentWeb的视频嗅探器

- 引用方式：引用aar文件即可
- 适用类型：M3U8、MP4、FLV（即使伪装成image等格式，也可以嗅探出来，如果结果不满意，请继承MySniffFilter覆写onFilter）
- 使用示例：

		SniffTool.getInstance((Activity) context)//绑定activity
			  .setSniffTimeout(50 * 1000)//设置嗅探总时限
                          .setJsTimeout(15 * 1000)//设置网页加载完毕后js执行的时限
                          .userAgent(DeviceManager.getUserAgent())//设置webview的user-agent
                          .setCallback(new SniffTool.Callback() {
                             @Override
                             public void onSuccess(SniffingVideo video) {
			     	    //因为我的项目只需要首个视频，因此只返回第一个，有需要请导入project修改
                                    GC.clean();
				    //HttpReferer包含了常用视频的header，包括了Bilivideo等，在嗅探过程会将cookie和header等都放在video中
				    //video还包含了contentType和视频类型等
                                    Map<String, String> headers = HttpReferer.getInstance(info.getUrl(), video.getUrl()).getMap();
			            if (!headers.isEmpty())
                                        video.addHeaders(headers);
                                    onGetVideo.onGetSuccess(video, currentTime, tname, false);
                                    cancel(true);
                            }
                            @Override
                            public void onFailed(int errorCode) {
                                   GC.clean();
                                   Log.e("SniffTool", "next url");
                                   sniff(iterator);
                            }
                         })
                         .setFilter(mfilter)
                         .target(finalWeb)
                         .start();
