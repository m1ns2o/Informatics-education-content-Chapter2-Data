# 비디오 파일 안내

이 폴더는 프레임레이트 비교를 위한 비디오 파일들을 저장합니다.

## 필요한 파일들

프레임레이트 비교를 위해서는 다양한 FPS의 동일한 영상이 필요합니다:

1. **sample-video-15fps.mp4** - 15fps 영상
2. **sample-video-30fps.mp4** - 30fps 영상  
3. **sample-video-60fps.mp4** - 60fps 영상
4. **sample-video-120fps.mp4** - 120fps 영상 (선택사항)

## 파일 생성 방법

기존 고프레임 영상을 다음과 같이 변환할 수 있습니다:

### FFmpeg 사용:
```bash
# 원본 영상에서 다양한 FPS로 변환
ffmpeg -i input-60fps.mp4 -r 15 -vf "fps=15" sample-video-15fps.mp4
ffmpeg -i input-60fps.mp4 -r 30 -vf "fps=30" sample-video-30fps.mp4
ffmpeg -i input-60fps.mp4 -r 60 -vf "fps=60" sample-video-60fps.mp4

# 간단한 움직임 테스트 영상 생성 (10초)
ffmpeg -f lavfi -i testsrc=duration=10:size=400x225:rate=60 -pix_fmt yuv420p test-60fps.mp4
ffmpeg -f lavfi -i testsrc=duration=10:size=400x225:rate=30 -pix_fmt yuv420p test-30fps.mp4
```

### 추천 영상 특징:
- 회전하는 객체나 빠른 움직임이 있는 영상 (차이를 명확히 보기 위해)
- 10-15초 정도의 짧은 반복 가능한 영상
- 해상도: 400x225 (16:9 비율)
- 무한 루프가 가능한 영상

## 임시 해결책

실제 비디오 파일이 없는 경우, Canvas 애니메이션으로 대체됩니다.