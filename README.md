# VideoStream
<h3>这里本地视频的推流采用FFmpeg</h3>
<a href="https://github.com/FFmpeg/FFmpeg">FFmpeg</a>是处理多媒体内容(如音频、视频、字幕和相关元数据)的库和工具的集合。<br/>
    ffmpeg基本使用方法可以参考<a href="https://www.jianshu.com/p/ddafe46827b7">https://www.jianshu.com/p/ddafe46827b7</a><br/><br/>
    
    ffmpeg推流的命令
    '''
    ffmpeg -re -i "输入视频（输入）" -vcodec copy -acodec aac -b:a 192k -f flv "直播链接（输出）"
    //以b站直播为例
    ffmpeg -re -i "1.mp4" -vcodec copy -acodec aac -b:a 192k -f flv "rtmp://dl.live-send.acg.tv/live-dl/你的直播码"
    '''
    -re 按照视频的FPS进行<br/>
    -i  指定输入文件地址，不一定是本地文件，可以是网络视频流，甚至可以是.m3u8文件。<br/>
    -vcodec copy    指定视频编码为复制
    -acodec aac     音频使用aac编码。后面的-b:a 192k是指定码率
    -f flv          指定输出格式，这个必须是flv才能推到直播服务器
    "直播链接（输出）"   最后是直播地址
    
<br/>
    python调用ffmpeg的库为ffmpy3，ffmpy3的<a href="https://ffmpy3.readthedocs.io/en/latest/index.html">相关文档</a> <br/><br/>
    python的ffmpy3的库实际上是对ffmpeg的封装
    
    
    from ffmpy3 import FFmpeg
    
    if __name__ == "__main__":
        ff = FFmpeg(
            inputs={'1.mp4' : '-re'},
            outputs={'rtmp://dl.live-send.acg.tv/live-dl/你的直播码' : '-vcodec copy -acodec aac -b:a 192k -f flv'}
        )
        print(ff.cmd)   # ffmpeg -re -i "1.mp4" -vcodec copy -acodec aac -b:a 192k -f flv "rtmp://dl.live-send.acg.tv/live-dl/你的直播码"
        ff.run()
