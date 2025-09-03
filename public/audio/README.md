# 오디오 파일 안내

이 폴더는 황금귀 테스트를 위한 오디오 파일들을 저장합니다.

## 필요한 파일들

실제 사용을 위해서는 다음 파일들을 이 폴더에 추가해야 합니다:

1. **sample-64kbps.mp3** - 저음질 샘플 (64kbps)
2. **sample-320kbps.mp3** - 고음질 샘플 (320kbps)

## 파일 생성 방법

기존 오디오 파일을 다음과 같이 변환할 수 있습니다:

### FFmpeg 사용:
```bash
# 320kbps로 변환
ffmpeg -i input.wav -b:a 320k sample-320kbps.mp3

# 64kbps로 변환  
ffmpeg -i input.wav -b:a 64k sample-64kbps.mp3
```

### 온라인 도구 사용:
- [Audio Converter](https://online-audio-converter.com/)
- [Convertio](https://convertio.co/mp3-converter/)

## 권장사항

- 클래식 음악이나 복잡한 악기 구성의 음악을 사용하면 비트레이트 차이가 더 명확하게 들립니다
- 파일 길이는 30초-1분 정도가 적당합니다
- 동일한 소스에서 비트레이트만 다르게 변환해야 정확한 비교가 가능합니다