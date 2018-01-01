# wlplayer v1.0
### Android ���ڣ�FFmpeg+OpenSL+OpenGL+Mediacodec ����Ƶ����SDK���ɲ������硢���غ͹㲥����ý��֧�ֵ�ǰ����ҳֱ���л�����Դ��֧����Ƶʵʱ��ͼ������ѡ��GPU���룬�����ٶȸ��죻���ֻ�֧��1080P��2K��4K�ȵ�����¶��ɲ��ţ���װ���ò���״̬�ص�������򵥡�
## APP Demo��ע����Ƶ������������èTV���㲥�����������й��㲥����
### [App Demo ���ص�ַ](https://pan.baidu.com/s/1cfjQAM)
![image](https://github.com/wanliyang1990/wlplayer/blob/master/imgs/1.png)
![image](https://github.com/wanliyang1990/wlplayer/blob/master/imgs/2.png)</br>
![image](https://github.com/wanliyang1990/wlplayer/blob/master/imgs/3.png)
![image](https://github.com/wanliyang1990/wlplayer/blob/master/imgs/4.png)</br>
![image](https://github.com/wanliyang1990/wlplayer/blob/master/imgs/5.png)</br>
![image](https://github.com/wanliyang1990/wlplayer/blob/master/imgs/6.png)</br>
![image](https://github.com/wanliyang1990/wlplayer/blob/master/imgs/7.png)</br>

## һ��API v1.0
#### 1���ص��ӿ�
    //׼���ò��ź�ص��˽ӿ�
    WlOnPreparedListener
    
    //��Ƶ���ػص��˽ӿ�
    WlOnLoadListener
    
    //��Ƶʱ���͵�ǰ����ʱ���ص��˽ӿ�
    WlOnInfoListener
    
    //��Ƶ����ص��˽ӿ�
    WlOnErrorListener
    
    //��Ƶ���Ž����ص��˽ӿ�
    WlOnCompleteListener
    
    //��Ƶ�����ص��˽ӿ�
    WlOnCutVideoImgListener
    
    //����ҳ�л�����Դʱ�ص��˽ӿڣ�stop(false)ʱ�����ڴ˽ӿڿ����������µĲ���Դ
    WlOnStopListener

#### 2������
    //���ò���Դ
    void setDataSource(String dataSource);
    
    //�����Ƿ񲥷���Ƶ���㲥��
    void setOnlyMusic(boolean onlyMusic)
    
    //������Ƶ��Ⱦglsurfaceview
    void setWlGlSurfaceView(WlGlSurfaceView wlGlSurfaceView)
    
    //׼�����ţ���Ӧ�ص��ӿڣ�
    void prepared()
    
    //׼���ú󣬿�ʼ����
    void start()
    
    //��ͣ
    void pause()
    
    //���ţ��������ͣ��
    void resume()
    
    //ֹͣ true�����ص�ֹͣ�ӿڣ�false:�ص�ֹͣ�ӿ�
    void stop(final boolean exit)
    
    //seek������ʱ�䣨���ǹؼ�֡�����ܻ���ּ����ӻ�����
    void seek(final int secds)
    
    //�õ��ܲ���ʱ��
    int getDuration()
     
    //�õ���Ƶ���
    int getVideoWidth()
    
    //�õ���Ƶ�߶�
    int getVideoHeight()
    
    //�õ�������
    int getAudioChannels()
    
    //������Ƶ���죨����������������������
    void setAudioChannels(int index)
    

## ������������
#### 1����Ӳ���
    <com.ywl5320.opengles.WlGlSurfaceView
        android:id="@+id/surfaceview"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
     
#### 2����������������


    private WlGlSurfaceView surfaceview;
    private WlPlayer wlPlayer;
    
    
    @Override
    public void onCreate(Bundle savedInstanceState) {
    
        wlPlayer = new WlPlayer();
        wlPlayer.setOnlyMusic(false); // true:���Ź㲥��false:������Ƶ
        wlPlayer.setDataSource(pathurl); //���ò���Դ
        wlPlayer.setWlGlSurfaceView(surfaceview); //���Ź㲥�ɲ�����Ƶ��Ⱦ����
    
    }

#### 3��׼������
    wlPlayer.prepared();
    
#### 4����ӻص���ע���������߳��У�

    //��Ƶ׼���ò���ʱ�ص�
    wlPlayer.setWlOnPreparedListener(new WlOnPreparedListener() {
            @Override
            public void onPrepared() {
        
                wlPlayer.start();//��ʼ����
                
            }
        });
        
    //���ػص�
    wlPlayer.setWlOnLoadListener(new WlOnLoadListener() {
            @Override
            public void onLoad(boolean load) {//true:������ false:�������
                
                Message message = Message.obtain();
                message.what = 1;
                message.obj = load;
                handler.sendMessage(message);
            }
        });
        
    //����ʱ����Ϣ�ص�
    wlPlayer.setWlOnInfoListener(new WlOnInfoListener() {
            @Override
            public void onInfo(WlTimeBean wlTimeBean) {//��ǰ����ʱ����ܵ�ʱ��
            
                Message message = Message.obtain();
                message.what = 2;
                message.obj = wlTimeBean;
                MyLog.d("nowTime is " +wlTimeBean.getCurrt_secds());
                handler.sendMessage(message);
            }
        });
        
    //����ص�
    wlPlayer.setWlOnErrorListener(new WlOnErrorListener() {
            @Override
            public void onError(int code, String msg) {
                MyLog.d("code:" + code + ",msg:" + msg);
                Message message = Message.obtain();
                message.obj = msg;
                message.what = 3;
                handler.sendMessage(message);
            }
        });
        
    //������ɻص�
    wlPlayer.setWlOnCompleteListener(new WlOnCompleteListener() {
            @Override
            public void onComplete() {
                MyLog.d("complete .....................");
                wlPlayer.stop(true);
            }
        });
        
    //��Ƶ�����ص�
    wlPlayer.setWlOnCutVideoImgListener(new WlOnCutVideoImgListener() {
            @Override
            public void onCutVideoImg(Bitmap bitmap) {
                Message message = Message.obtain();
                message.what = 4;
                message.obj = bitmap;
                handler.sendMessage(message);
            }
        });
        
    //ֹͣ���Żص�����wlPlayer.stop(false)������²Ż�ص��������������ɴ����л�����Դ������
    wlPlayer.setWlOnStopListener(new WlOnStopListener() {
            @Override
            public void onStop() {
                Message message = Message.obtain();
                message.what = 3;
                handler.sendMessage(message);
            }
        });
        

## ����ﵽ������������ѡ�����һ��Ŷ~
<img display:inline-block float:left width="250" height="298" src="https://github.com/wanliyang1990/wlplayer/blob/master/imgs/zan_ali.png"/>
<img display:inline-block float:left width="250" height="298" src="https://github.com/wanliyang1990/wlplayer/blob/master/imgs/zan_wx.png"/>


## TODO
### �����Ժ������Ż�
</br>

## ע
#### ��ǰ������FFmpeg-3.4, AS-3.0�� NDK-14b��С���ֻ�2A
#### CPU��arm(only)
#### 2018-01-01 happy new year��
</br>

## Create by:ywl5320
</br>

