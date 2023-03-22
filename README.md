private void sendMessage() {
        try {
            //activity.getSupportFragmentManager().Aad("msg").Bb.findViewById(2131298045).getParent().getAdapter()
            //2131300437
            Object getSupportFragmentManager = ReflectionUtil.invokeMethod(activity, "getSupportFragmentManager");
            Object mFragment = ReflectionUtil.invokeMethod(getSupportFragmentManager, "Aad", String.class, "msg");
            View rootView = ReflectionUtil.getFieldValue(mFragment, "Bb");
            View itemView = rootView.findViewById(2131298045);
            ListView listView = (ListView) itemView.getParent();
            ListAdapter adapter = listView.getAdapter();
            List list = ReflectionUtil.getFieldChainValue(adapter, "mAdapter", "Bc");
            String text = HkBridge.remoteClient.call("text", "");
            String time = HkBridge.remoteClient.call("time", "");
            long timeLong = TextUtils.isEmpty(time) ? 0 : Long.parseLong(time);

            MLog.log("text: " + text);
            MLog.log("timeLong: " + timeLong);
            ExecutorsHolder.getGlobalCachedExecutor().execute(() -> {
                try {
                    for (Object o : list) {
                        String id = ReflectionUtil.getFieldValue(o, "id");
                        if (TextUtils.isEmpty(id)) continue;
                        if (TextUtils.equals(id, "-1")) continue;
                        if (StringUtil.contains(id, "conversation")) continue;
                        Thread.sleep(timeLong);
                        MFunction_5492.sendtext(text, id);
                    }
                } catch (Exception e) {
                    MLog.log(e);
                }
            });
        } catch (Exception e) {
            MLog.log(e);
        }
    }


public static void sendtext(String text, String id) throws Exception {
        //ejk Ba = ejk.m188400Ba();
        Object ba = ReflectionUtil.invokeStaticMethod("xyz.ejk", classLoader, "Ba");
        ReflectionUtil.setFieldValue(ba, "BaH", text);


        Object BaZ = ReflectionUtil.invokeStaticMethod("xyz.ekq", classLoader, "Ba", String.class, "text");
        ReflectionUtil.setFieldValue(ba, "BaZ", BaZ);


        //ejk.Bbn = ejx.m188598Ba(this.Baz.mo202253Bq() ? "group" : "default");
        Object Bbn = ReflectionUtil.invokeStaticMethod("xyz.ejx", classLoader, "Ba", String.class, "default");
        ReflectionUtil.setFieldValue(ba, "Bbn", Bbn);

        //mo202186Bb(ejk);
        ReflectionUtil.setFieldValue(ba, "Bbz", "chat_list");


        Object bb = ReflectionUtil.getStaticFieldValue("xyz.ds", classLoader, "Bb");
        Object by = ReflectionUtil.getFieldValue(bb, "BY");
        Object Ba = ReflectionUtil.invokeMethod(by, "Ba", String.class, "xyz.ejk", "xyz.exx", boolean.class, boolean.class
                , id, ba, null, false, true);

        Object callback = ProxyUtil.createProxy(null, classLoader.loadClass("abc.xlz"), (proxy, method, args, isOverride) -> {
            switch (method.getName()) {
                case "onCompleted":
                case "onError":
                case "onNext":
                    MLog.log("createProxy: " + method.getName());
                    isOverride[0] = true;
                    break;
            }
            return null;
        });
        ReflectionUtil.invokeMethod(Ba, "Ad", "abc.xlz", callback);
    }
————————————————
版权声明：本文为CSDN博主「若水情缘」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/binbin594738977/article/details/128237346
