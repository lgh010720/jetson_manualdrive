### dxl을 이용하여 바퀴를 움직여 jetson이 주행하고 카메라를 이용하여 영상을 촬영, 출력, 저장하는 프로그램

start, end1, time1 : 코드 실행 시간

src, dst1, writer1 : 영상 촬영, 출력

video, save : 영상 저장

if (mx.kbhit()) : 키보드입력, f: 전진, b : 후진, l : 좌회전, r : 우회전, s : 정지

gol1, gol2 : 바퀴속도 

가속 및 감속

    if(goal1>vel1) vel1 += 5;
        else if(goal1<vel1) vel1 -= 5;
        else vel1 = goal1;

        if(goal2>vel2) vel2 += 5;
        else if(goal2<vel2) vel2 -= 5;
        else vel2 = goal2;
        
## 실행영상
https://www.youtube.com/watch?v=JgmP-8WQvxE
