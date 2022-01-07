# sniffWebkit
基于AgentWeb的视频嗅探器
# sniffWebkit
基于AgentWeb的视频嗅探器

- 引用方式：引用aar文件即可
	
- 使用示例：

		SniffTool.getInstance((Activity) context)
			  .setSniffTimeout(50 * 1000)
                          .setJsTimeout(15 * 1000)
                          .userAgent(DeviceManager.getUserAgent())
                          .setCallback(new SniffTool.Callback() {
                             @Override
                             public void onSuccess(SniffingVideo video) {
                                    GC.clean();
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
