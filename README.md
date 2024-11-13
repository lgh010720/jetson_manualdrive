### dxl을 이용하여 바퀴를 움직여 jetson이 주행하고 카메라를 이용하여 영상을 촬영, 출력, 저장하는 프로그램

코드 실행 시간

    struct timeval start,end1;
    double diff1;
    gettimeofday(&start,NULL);
    gettimeofday(&end1,NULL);
    diff1 = end1.tv_sec + end1.tv_usec / 1000000.0 - start.tv_sec - start.tv_usec / 1000000.0;
    
영상 촬영, 출력

    string src = "nvarguscamerasrc sensor-id=0 ! \
	video/x-raw(memory:NVMM), width=(int)640, height=(int)360, \
    format=(string)NV12, framerate=(fraction)30/1 ! \
	nvvidconv flip-method=0 ! video/x-raw, \
	width=(int)640, height=(int)360, format=(string)BGRx ! \
	videoconvert ! video/x-raw, format=(string)BGR ! appsink"; 

    VideoCapture source(src, CAP_GSTREAMER); 
    if (!source.isOpened()){ cout << "Camera error" << endl; return -1; }

    string dst1 = "appsrc ! videoconvert ! video/x-raw, format=BGRx ! \
	nvvidconv ! nvv4l2h264enc insert-sps-pps=true ! \
	h264parse ! rtph264pay pt=96 ! \
	udpsink host=203.234.58.160 port=8001 sync=false";
    
    VideoWriter writer1(dst1,0, (double)30,Size(640,360),true);
    if(!writer1.isOpened()) {cerr<<"Writer open failed!"<<endl; return -1;}
    Mat frame, gray;

영상 저장

    string video = "output_video.avi";
    VideoWriter save_video(video, VideoWriter::fourcc('M', 'J', 'P', 'G'), 30, Size(640, 360), true);
    if (!save_video.isOpened()) {
        cerr << "Failed to open video writer!" << endl; return -1;}

        save_video << frame;

키보드입력, f: 전진, b : 후진, l : 좌회전, r : 우회전, s : 정지, ctrl+c : 종료

    if (mx.kbhit())
        {
            char c = mx.getch();
            switch(c)
            {
            case 's': goal1 = 0; goal2 = 0; break;
            case ' ': goal1 = 0; goal2 = 0; break;
            case 'f': goal1 = 50; goal2 = -50; break;
            case 'b': goal1 = -50; goal2 = 50; break;
            case 'l': goal1 = -30; goal2 = -50; break;
            case 'r': goal1 = 50; goal2 = 30; break;
            default : goal1 = 0; goal2 = 0; break;
            }         
        }
    if (ctrl_c_pressed) break;         
        usleep(20*1000);  


가속 및 감속

    if(goal1>vel1) vel1 += 5;
        else if(goal1<vel1) vel1 -= 5;
        else vel1 = goal1;

        if(goal2>vel2) vel2 += 5;
        else if(goal2<vel2) vel2 -= 5;
        else vel2 = goal2;
        
## 실행영상
https://www.youtube.com/watch?v=JgmP-8WQvxE
