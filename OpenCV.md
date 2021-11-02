
```python 
import cv2

def main(video_name):
    video_file = settings.VIDEO_FILE_DIR + video_name + '.mp4'
    frame_dir = settings.VIDEO_FRAME_DIR + video_name + '/'

    if not (os.path.exists(frame_dir)):
        os.mkdir(frame_dir)

    vid_cap = cv2.VideoCapture(video_file)  # 读入视频文件

    count = 1
    if vid_cap.isOpened():  # 判断是否正常打开
        success, frame = vid_cap.read()
    else:
        success = False

    time_interval = vid_cap.get(5) * settings.SPLIT_DURATION  # 视频帧计数间隔频率 = 帧率 * 切片时间间隔

    while success:  # 循环读取视频帧
        success, frame = vid_cap.read()
        if count % time_interval == 0:  # 每隔timeF帧进行存储操作
            print(count)
            cv2.imencode('.jpg', frame)[1].tofile(frame_dir + str(count).zfill(10) + '.jpg')  # 存储为图像
        count = count + 1
        cv2.waitKey(1)
    vid_cap.release()
```
