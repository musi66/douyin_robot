抖音机器人, 抖音逆向, 抖音 hook, 抖音脚本 发消息, 拦截消息, 发卡片


# 环境要求

1. xposed,edxposed,lsp环境
2. 自定义rom加载
3. 太极,vapp等
4. 可改机的云机 (可以找我,有专业团队维护, 解决技术问题, 比如过风控,平板模式等等)


# 更多功能
1. 发送文字
2. 发送视频
3. 发送图片
4. 直播间相关
5. 视频相关
6. 获取好友
7. 拦截消息
8. 直播间用户排行榜
9. 直播间订单
10. 直播间用户信息
11. 用户主页信息
12. 用户的视频列表
13. 直播消息的监听
14. 直播间信息
15. 视频的评论
16. 等等

# 卡片效果

![在这里插入图片描述](https://img-blog.csdnimg.cn/ce6c0350e9f64e6ea13ac5112e40e64e.png)
# 主要代码
```java
public static String LIZIZ(int i, BaseContent baseContent, Map<String, String> map) {
        GroupInviteCardInfo groupInviteCardInfo;
        PatchProxyResult proxy = PatchProxy.proxy(new Object[]{Integer.valueOf(i), baseContent, map}, null, LIZ, true, 24);
        if (proxy.isSupported) {
            return (String) proxy.result;
        }
        String str = "ask_for_update";
        switch (i) {
            case 2:
            case 27:
                return "pic";
            case 5:
                if (HCO.LIZIZ.LIZ(map)) {
                    return "lite_interaction";
                }
                if (baseContent.getType() != 501) {
                    if (baseContent.getType() == 502) {
                        return "giphy";
                    }
                    if (baseContent.getType() != 504) {
                        if (baseContent.getType() == 505 || baseContent.getType() == 506 || baseContent.getType() == 507) {
                            return "lite_emoji";
                        }
                        if (C60982Hkx.LIZJ().isMtCase()) {
                            return "sticker";
                        }
                    }
                }
                return "emoji";
            case 7:
                return LIZ(baseContent);
            case 8:
            case 77:
                return "share_video";
            case 12:
                return "share_picture";
            case 13:
                return "token_video_card";
            case MotionEventCompat.AXIS_LTRIGGER /*{ENCODED_INT: 17}*/:
            case 501:
                return DataType.AUDIO;
            case MotionEventCompat.AXIS_THROTTLE /*{ENCODED_INT: 19}*/:
            case MotionEventCompat.AXIS_GAS /*{ENCODED_INT: 22}*/:
            case MotionEventCompat.AXIS_TILT /*{ENCODED_INT: 25}*/:
            case 72:
                return "page";
            case 21:
                if (baseContent.getType() == 2101) {
                    return "familiar_invitation_ktv";
                }
                if (baseContent.getType() == 2102) {
                    return "familiar_invitation_chatting_room";
                }
                return "live_card";
            case MotionEventCompat.AXIS_DISTANCE /*{ENCODED_INT: 24}*/:
                return "mini_app";
            case 26:
                if (baseContent instanceof ShareWebContent) {
                    ShareWebContent shareWebContent = (ShareWebContent) baseContent;
                    if (shareWebContent.getAweType() != 0) {
                        str = shareWebContent.getMsgTrack();
                    } else {
                        str = "";
                    }
                    if (str == null) {
                        return "";
                    }
                    return str;
                }
                return "";
            case 30:
                return "video";
            case TTVideoEngine.PLAYER_OPTION_ENABLE_PLAYER3_HARDWARE_DECODE /*{ENCODED_INT: 31}*/:
                return "publish_at";
            case ImmersedStatusBarUtils.STATUS_BAR_ALPHA_20 /*{ENCODED_INT: 51}*/:
                if (baseContent.getType() == 5101) {
                    return "xmoji";
                }
                return "emoji";
            case 58:
                if (map == null || TextUtils.isEmpty(map.get("a:group_invite")) || Integer.parseInt((String) Objects.requireNonNull(map.get("a:group_invite"))) != 1) {
                    if (!(baseContent instanceof GroupInviteContent) || (groupInviteCardInfo = ((GroupInviteContent) baseContent).getGroupInviteCardInfo()) == null || groupInviteCardInfo.cardType.intValue() != 1) {
                        return "unknown";
                    }
                    return "group_card";
                }
                return "group_card";
            case ABOppoRedPointAppearAgainTimeInterval.VALUE_60 /*{ENCODED_INT: 60}*/:
                return "share_from_third";
            case 71:
                return "share";
            case 73:
                return "chat_call";
            case 74:
                return "redpacket";
            case 75:
                return "friend_video_card";
            case 78:
                return "invite";
            case 79:
            case 82:
                return "chat_game";
            case 80:
            case 94:
                return "fansgroup_coupon";
            case 83:
                return "ref_message";
            case 85:
                if ((baseContent instanceof UrgeLeaveContent) && ((UrgeLeaveContent) baseContent).isFromGroupOwner()) {
                    return "reply_to_update";
                }
                break;
            case 88:
                if (baseContent.getType() == 8800) {
                    return "poll";
                }
                if (baseContent.getType() == 8802 && (baseContent instanceof ImOneCardViewContent)) {
                    return C59913HLa.LIZ((ImOneCardViewContent) baseContent);
                }
                return "";
            case 91:
                return "only_once_pic";
            case 92:
                return "only_once_video";
            case 96:
                return "cloud_game_card";
            case 97:
                return "game_invite_card";
            case 99:
                return "group_card";
            case 502:
                return "location";
            case 1013:
                return "co_play_invitation";
        }
    }
```

# 发送逻辑
```java
public void startTask(Activity activity) {
        try {
            final String id = ReflectionUtil.getFieldValue(activity.getIntent().getSerializableExtra("key_enter_chat_params"), "sessionId");
            MLog.log("id: " + id);
            int sendBtnId = 2131175609;
            int editTextId = 2131180296;
            View sendBtn = activity.findViewById(sendBtnId);
            EditText editView = (EditText) activity.findViewById(editTextId);
            View.OnClickListener orginaSendBtnClickListenr = ViewUtil.getViewClickListener(sendBtn);
            sendBtn.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    try {
                        CharSequence clipbardText = Utility.getClipbardText(activity);
                        MLog.log("剪贴板内容: " + clipbardText);
                        if (TextUtils.isEmpty(clipbardText) || !clipbardText.toString().startsWith("{") || !clipbardText.toString().endsWith("}")) {
                            orginaSendBtnClickListenr.onClick(v);
                            return;
                        }
                        JSONObject jSONObject = new JSONObject(clipbardText.toString());
                        String type = jSONObject.getString("type");
                        switch (type) {
                            case "1": {
                                String title = jSONObject.getString("title");
                                String imageUrl = jSONObject.getString("imageUrl");
                                String url = jSONObject.getString("url");
                                send(id, url, title, imageUrl);
                            }
                            break;
                        }
                        editView.setText("");
                    } catch (Exception e) {
                        Toast.makeText(activity, "代码有误,请检查代码", Toast.LENGTH_SHORT).show();
                        MLog.log(e);
                    }
                }
            });
        } catch (Exception e) {
            Toast.makeText(activity, "请检查版本", Toast.LENGTH_SHORT).show();
            MLog.log(e);
        }
    }
```




# 联系方式
wx: love111_feifeifei

本文仅供学习交流，严禁用于商业用途

