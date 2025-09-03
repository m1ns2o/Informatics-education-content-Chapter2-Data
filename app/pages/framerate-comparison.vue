<template>
	<div class="container mx-auto px-4 py-8">
		<div class="max-w-6xl mx-auto">
			<div class="text-center mb-8">
				<h1 class="text-3xl font-bold text-gray-900 mb-4">프레임레이트 비교</h1>
				<p class="text-lg text-gray-600">
					다양한 프레임레이트의 영상을 비교하며 차이점을 학습하세요
				</p>
			</div>

			<!-- 프레임레이트 설정 -->
			<UCard class="mb-8">
				<template #header>
					<h3 class="text-xl font-semibold flex items-center gap-2">
						<UIcon name="i-lucide-settings" class="w-5 h-5 text-blue-500" />
						프레임레이트 설정
					</h3>
				</template>

				<div class="space-y-6">
					<div class="grid grid-cols-1 md:grid-cols-2 gap-6">
						<div>
							<label class="block text-sm font-medium mb-3">
								왼쪽 영상 FPS: {{ leftFps }}fps
							</label>
							<input
								type="range"
								v-model="leftFps"
								:min="5"
								:max="120"
								:step="5"
								class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer slider mb-2"
							/>
							<div class="flex justify-between text-xs text-gray-500">
								<span>5fps</span>
								<span>60fps</span>
								<span>120fps</span>
							</div>
						</div>

						<div>
							<label class="block text-sm font-medium mb-3">
								오른쪽 영상 FPS: {{ rightFps }}fps
							</label>
							<input
								type="range"
								v-model="rightFps"
								:min="5"
								:max="120"
								:step="5"
								class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer slider mb-2"
							/>
							<div class="flex justify-between text-xs text-gray-500">
								<span>5fps</span>
								<span>60fps</span>
								<span>120fps</span>
							</div>
						</div>
					</div>

					<div class="flex justify-center gap-4">
						<UButton
							@click="toggleAnimation"
							:icon="isPlaying ? 'i-lucide-pause' : 'i-lucide-play'"
							:label="isPlaying ? '일시정지' : '재생'"
							size="lg"
						/>
						<UButton
							@click="toggleMode"
							:icon="useVideoMode ? 'i-lucide-video' : 'i-lucide-brush'"
							:label="useVideoMode ? '비디오 모드' : '캔버스 모드'"
							variant="outline"
							size="lg"
						/>
					</div>
				</div>
			</UCard>

			<div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
				<!-- 왼쪽 애니메이션 -->
				<UCard>
					<template #header>
						<h3 class="text-xl font-semibold flex items-center justify-between">
							<div class="flex items-center gap-2">
								<UIcon name="i-lucide-video" class="w-5 h-5 text-green-500" />
								프레임레이트: {{ leftFps }}fps
							</div>
							<UBadge
								:color="
									leftFps >= 60
										? 'success'
										: leftFps >= 30
										? 'warning'
										: 'error'
								"
								variant="soft"
							>
								{{ getFpsQuality(leftFps) }}
							</UBadge>
						</h3>
					</template>

					<div class="space-y-4">
						<div
							class="border border-gray-200 rounded-lg overflow-hidden bg-gray-900 aspect-video relative"
						>
							<video
								v-if="useVideoMode"
								ref="leftVideo"
								:src="getVideoSource(leftFps)"
								class="w-full h-full object-cover"
								muted
								loop
								:style="{ display: 'block' }"
								@loadedmetadata="onVideoLoaded('left')"
							></video>
							<canvas
								v-else
								ref="leftCanvas"
								width="400"
								height="225"
								class="w-full h-full"
							></canvas>
							<div
								class="absolute top-2 right-2 text-xs text-white bg-black bg-opacity-50 px-2 py-1 rounded"
							>
								{{ useVideoMode ? "Video Mode" : "Canvas Mode" }}
							</div>
						</div>

						<div class="text-sm text-gray-600">
							<p><strong>특징:</strong></p>
							<ul class="list-disc list-inside mt-2 space-y-1">
								<li v-for="feature in getFeatures(leftFps)" :key="feature">
									{{ feature }}
								</li>
							</ul>
						</div>
					</div>
				</UCard>

				<!-- 오른쪽 애니메이션 -->
				<UCard>
					<template #header>
						<h3 class="text-xl font-semibold flex items-center justify-between">
							<div class="flex items-center gap-2">
								<UIcon name="i-lucide-video" class="w-5 h-5 text-orange-500" />
								프레임레이트: {{ rightFps }}fps
							</div>
							<UBadge
								:color="
									rightFps >= 60
										? 'success'
										: rightFps >= 30
										? 'warning'
										: 'error'
								"
								variant="soft"
							>
								{{ getFpsQuality(rightFps) }}
							</UBadge>
						</h3>
					</template>

					<div class="space-y-4">
						<div
							class="border border-gray-200 rounded-lg overflow-hidden bg-gray-900 aspect-video relative"
						>
							<video
								v-if="useVideoMode"
								ref="rightVideo"
								:src="getVideoSource(rightFps)"
								class="w-full h-full object-cover"
								muted
								loop
								:style="{ display: 'block' }"
								@loadedmetadata="onVideoLoaded('right')"
							></video>
							<canvas
								v-else
								ref="rightCanvas"
								width="400"
								height="225"
								class="w-full h-full"
							></canvas>
							<div
								class="absolute top-2 right-2 text-xs text-white bg-black bg-opacity-50 px-2 py-1 rounded"
							>
								{{ useVideoMode ? "Video Mode" : "Canvas Mode" }}
							</div>
						</div>

						<div class="text-sm text-gray-600">
							<p><strong>특징:</strong></p>
							<ul class="list-disc list-inside mt-2 space-y-1">
								<li v-for="feature in getFeatures(rightFps)" :key="feature">
									{{ feature }}
								</li>
							</ul>
						</div>
					</div>
				</UCard>
			</div>

			<!-- 프레임레이트 비교 분석 -->
			<UCard class="mt-8">
				<template #header>
					<h3 class="text-xl font-semibold flex items-center gap-2">
						<UIcon name="i-lucide-bar-chart" class="w-5 h-5 text-purple-500" />
						현재 설정 비교 분석
					</h3>
				</template>

				<div class="grid grid-cols-1 md:grid-cols-2 gap-6">
					<div>
						<h4 class="font-semibold text-lg mb-3">{{ leftFps }}fps (왼쪽)</h4>
						<div class="space-y-2 text-sm">
							<div class="flex justify-between">
								<span>부드러움:</span>
								<div class="flex">
									<div
										v-for="i in 5"
										:key="i"
										class="w-3 h-3 mr-1 rounded"
										:class="
											i <= getSmoothness(leftFps)
												? 'bg-green-400'
												: 'bg-gray-200'
										"
									></div>
								</div>
							</div>
							<div class="flex justify-between">
								<span>배터리 효율:</span>
								<div class="flex">
									<div
										v-for="i in 5"
										:key="i"
										class="w-3 h-3 mr-1 rounded"
										:class="
											i <= getBatteryEfficiency(leftFps)
												? 'bg-blue-400'
												: 'bg-gray-200'
										"
									></div>
								</div>
							</div>
							<div class="flex justify-between">
								<span>게임 적합성:</span>
								<div class="flex">
									<div
										v-for="i in 5"
										:key="i"
										class="w-3 h-3 mr-1 rounded"
										:class="
											i <= getGamingSuitability(leftFps)
												? 'bg-orange-400'
												: 'bg-gray-200'
										"
									></div>
								</div>
							</div>
						</div>
					</div>

					<div>
						<h4 class="font-semibold text-lg mb-3">
							{{ rightFps }}fps (오른쪽)
						</h4>
						<div class="space-y-2 text-sm">
							<div class="flex justify-between">
								<span>부드러움:</span>
								<div class="flex">
									<div
										v-for="i in 5"
										:key="i"
										class="w-3 h-3 mr-1 rounded"
										:class="
											i <= getSmoothness(rightFps)
												? 'bg-green-400'
												: 'bg-gray-200'
										"
									></div>
								</div>
							</div>
							<div class="flex justify-between">
								<span>배터리 효율:</span>
								<div class="flex">
									<div
										v-for="i in 5"
										:key="i"
										class="w-3 h-3 mr-1 rounded"
										:class="
											i <= getBatteryEfficiency(rightFps)
												? 'bg-blue-400'
												: 'bg-gray-200'
										"
									></div>
								</div>
							</div>
							<div class="flex justify-between">
								<span>게임 적합성:</span>
								<div class="flex">
									<div
										v-for="i in 5"
										:key="i"
										class="w-3 h-3 mr-1 rounded"
										:class="
											i <= getGamingSuitability(rightFps)
												? 'bg-orange-400'
												: 'bg-gray-200'
										"
									></div>
								</div>
							</div>
						</div>
					</div>
				</div>
			</UCard>

			<!-- 프레임레이트 활용 가이드 -->
			<UCard class="mt-8">
				<template #header>
					<h3 class="text-xl font-semibold flex items-center gap-2">
						<UIcon name="i-lucide-book-open" class="w-5 h-5 text-indigo-500" />
						프레임레이트 활용 가이드
					</h3>
				</template>

				<div class="grid grid-cols-1 md:grid-cols-3 gap-6">
					<div>
						<h4 class="font-semibold text-red-600 mb-3">저프레임 (15-30fps)</h4>
						<div class="text-sm space-y-2">
							<p><strong>특징:</strong> 약간 끊어지는 느낌, 배터리 효율 높음</p>
							<p><strong>적합한 용도:</strong></p>
							<ul class="list-disc list-inside mt-1 space-y-1">
								<li>영화 (24fps 표준)</li>
								<li>일반 동영상 시청</li>
								<li>프레젠테이션</li>
								<li>배터리 절약이 중요한 경우</li>
							</ul>
						</div>
					</div>

					<div>
						<h4 class="font-semibold text-yellow-600 mb-3">
							중간프레임 (30-60fps)
						</h4>
						<div class="text-sm space-y-2">
							<p><strong>특징:</strong> 자연스러운 움직임, 균형적인 성능</p>
							<p><strong>적합한 용도:</strong></p>
							<ul class="list-disc list-inside mt-1 space-y-1">
								<li>TV 방송 (30fps)</li>
								<li>YouTube 동영상</li>
								<li>일반적인 게임 플레이</li>
								<li>스마트폰 녹화</li>
							</ul>
						</div>
					</div>

					<div>
						<h4 class="font-semibold text-green-600 mb-3">고프레임 (60fps+)</h4>
						<div class="text-sm space-y-2">
							<p><strong>특징:</strong> 매우 부드러운 움직임, 높은 반응성</p>
							<p><strong>적합한 용도:</strong></p>
							<ul class="list-disc list-inside mt-1 space-y-1">
								<li>게임 (특히 FPS, 액션)</li>
								<li>스포츠 중계</li>
								<li>VR/AR 콘텐츠</li>
								<li>고급 디스플레이 활용</li>
							</ul>
						</div>
					</div>
				</div>
			</UCard>

			<!-- 실험 제안 -->
			<UCard class="mt-8">
				<template #header>
					<h3 class="text-xl font-semibold flex items-center gap-2">
						<UIcon name="i-lucide-flask" class="w-5 h-5 text-pink-500" />
						추천 실험
					</h3>
				</template>

				<div class="grid grid-cols-1 md:grid-cols-2 gap-6">
					<UButton
						variant="outline"
						size="lg"
						block
						@click="setComparison(5, 60)"
					>
						극단적 차이 비교 (5fps vs 60fps)
					</UButton>

					<UButton
						variant="outline"
						size="lg"
						block
						@click="setComparison(15, 60)"
					>
						뚜렷한 차이 비교 (15fps vs 60fps)
					</UButton>

					<UButton
						variant="outline"
						size="lg"
						block
						@click="setComparison(30, 60)"
					>
						일반적 차이 비교 (30fps vs 60fps)
					</UButton>

					<UButton
						variant="outline"
						size="lg"
						block
						@click="setComparison(60, 120)"
					>
						고급 차이 비교 (60fps vs 120fps)
					</UButton>
				</div>
			</UCard>

			<PageNavigation
				:previous-page="{ path: '/image-comparison', label: '이미지 비교' }"
			/>
		</div>
	</div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, watch } from "vue";
import PageNavigation from "../components/PageNavigation.vue";

const leftFps = ref(15);
const rightFps = ref(60);
const isPlaying = ref(true);
const useVideoMode = ref(false);

const leftCanvas = ref<HTMLCanvasElement | null>(null);
const rightCanvas = ref<HTMLCanvasElement | null>(null);
const leftVideo = ref<HTMLVideoElement | null>(null);
const rightVideo = ref<HTMLVideoElement | null>(null);

let animationId: number;
const masterFps = 60; // 기준 프레임레이트
let frameCount = ref(0);
const videosLoaded = ref({ left: false, right: false });

// 각 캔버스의 마지막 렌더링 상태 저장
const canvasStates = ref({
	left: { lastRenderedFrame: -1, lastProgress: 0 },
	right: { lastRenderedFrame: -1, lastProgress: 0 },
});

const getFpsQuality = (fps: number) => {
	if (fps >= 90) return "최고급";
	if (fps >= 60) return "고품질";
	if (fps >= 30) return "표준";
	if (fps >= 24) return "기본";
	return "저품질";
};

const getFeatures = (fps: number) => {
	if (fps >= 90)
		return [
			"극도로 부드러운 움직임",
			"높은 반응성",
			"게임/VR에 최적",
			"높은 전력 소모",
		];
	if (fps >= 60)
		return [
			"매우 부드러운 움직임",
			"좋은 반응성",
			"게임에 적합",
			"중간 전력 소모",
		];
	if (fps >= 30)
		return ["자연스러운 움직임", "일반적인 용도에 적합", "적당한 전력 소모"];
	if (fps >= 24) return ["영화 표준", "적당한 부드러움", "낮은 전력 소모"];
	return ["약간 끊어지는 느낌", "기본적인 움직임", "매우 낮은 전력 소모"];
};

const getSmoothness = (fps: number) =>
	Math.min(5, Math.max(1, Math.floor(fps / 24)));
const getBatteryEfficiency = (fps: number) =>
	Math.max(1, 6 - Math.floor(fps / 24));
const getGamingSuitability = (fps: number) =>
	Math.min(5, Math.max(1, Math.floor(fps / 18)));

// 비디오 소스 결정 함수
const getVideoSource = (fps: number) => {
	// 가장 가까운 FPS 파일 매칭
	if (fps <= 15) return "/video/sample-video-15fps.mp4";
	if (fps <= 30) return "/video/sample-video-30fps.mp4";
	if (fps <= 60) return "/video/sample-video-60fps.mp4";
	return "/video/sample-video-120fps.mp4";
};

// 비디오 로드 완료 처리
const onVideoLoaded = (side: "left" | "right") => {
	videosLoaded.value[side] = true;
	if (videosLoaded.value.left && videosLoaded.value.right) {
		// 두 비디오 모두 로드되면 동기화
		syncVideos();
	}
};

// 비디오 동기화
const syncVideos = () => {
	if (!leftVideo.value || !rightVideo.value) return;

	leftVideo.value.currentTime = 0;
	rightVideo.value.currentTime = 0;

	if (isPlaying.value) {
		leftVideo.value.play();
		rightVideo.value.play();
	}
};

// 모드 전환
const toggleMode = () => {
	useVideoMode.value = !useVideoMode.value;
	if (useVideoMode.value) {
		// 비디오 모드로 전환 시 Canvas 애니메이션 중지
		stopAnimations();
		// 비디오 파일 체크 및 폴백
		checkVideoFiles();
	} else {
		// 캔버스 모드로 전환 시 비디오 중지
		if (leftVideo.value) leftVideo.value.pause();
		if (rightVideo.value) rightVideo.value.pause();
		if (isPlaying.value) startAnimations();
	}
};

// 비디오 파일 존재 체크
const checkVideoFiles = async () => {
	try {
		const testVideo = document.createElement("video");
		testVideo.src = "/video/sample-video-30fps.mp4";

		await new Promise((resolve, reject) => {
			testVideo.onloadedmetadata = resolve;
			testVideo.onerror = reject;
			setTimeout(() => reject(new Error("timeout")), 3000); // 3초 타임아웃
		});
	} catch {
		// 비디오 파일이 없으면 캔버스 모드로 복귀
		useVideoMode.value = false;
		alert("비디오 파일을 찾을 수 없습니다. 캔버스 모드를 사용합니다.");
	}
};

const setComparison = (left: number, right: number) => {
	leftFps.value = left;
	rightFps.value = right;

	if (useVideoMode.value) {
		// 비디오 모드에서는 소스 변경 후 동기화
		setTimeout(() => syncVideos(), 100);
	} else {
		// 캔버스 모드에서는 프레임 카운터와 상태 리셋
		frameCount.value = 0;
		canvasStates.value.left.lastRenderedFrame = -1;
		canvasStates.value.left.lastProgress = 0;
		canvasStates.value.right.lastRenderedFrame = -1;
		canvasStates.value.right.lastProgress = 0;
	}
};

const toggleAnimation = () => {
	isPlaying.value = !isPlaying.value;

	if (useVideoMode.value) {
		// 비디오 모드
		if (isPlaying.value) {
			leftVideo.value?.play();
			rightVideo.value?.play();
		} else {
			leftVideo.value?.pause();
			rightVideo.value?.pause();
		}
	} else {
		// 캔버스 모드
		if (isPlaying.value) {
			startAnimations();
		} else {
			stopAnimations();
		}
	}
};

const drawFrame = (
	canvas: HTMLCanvasElement,
	fps: number,
	masterFrameNumber: number,
	side: "left" | "right"
) => {
	const ctx = canvas.getContext("2d");
	if (!ctx) return;

	const width = canvas.width;
	const height = canvas.height;

	// 해당 FPS에서의 프레임 간격 계산 (60fps 기준)
	const frameInterval = Math.round(masterFps / fps);

	// 현재 프레임이 새로 그려야 하는 프레임인지 확인
	const shouldUpdate = masterFrameNumber % frameInterval === 0;

	let progress: number;

	if (shouldUpdate) {
		// 실제 시간 기반으로 진행률 계산 (속도는 항상 동일)
		// 2도씩 회전하여 30초에 한 바퀴 (360도)
		progress = (masterFrameNumber * 2) % 360;

		// 상태 저장
		canvasStates.value[side].lastRenderedFrame = masterFrameNumber;
		canvasStates.value[side].lastProgress = progress;
	} else {
		// 이전 프레임의 상태를 유지 (프레임 건너뛰기)
		progress = canvasStates.value[side].lastProgress;
	}

	// 배경 지우기 (매번 새로 그려야 함)
	ctx.fillStyle = "#1a1a1a";
	ctx.fillRect(0, 0, width, height);

	// 회전하는 정육각형
	const centerX = width / 2;
	const centerY = height / 2;
	const radius = 80;

	ctx.save();
	ctx.translate(centerX, centerY);
	ctx.rotate((progress * Math.PI) / 180);

	// 정육각형 그리기
	ctx.beginPath();
	for (let i = 0; i < 6; i++) {
		const angle = (i * Math.PI) / 3;
		const x = Math.cos(angle) * radius;
		const y = Math.sin(angle) * radius;
		if (i === 0) ctx.moveTo(x, y);
		else ctx.lineTo(x, y);
	}
	ctx.closePath();

	// 그라데이션
	const gradient = ctx.createLinearGradient(-radius, -radius, radius, radius);
	gradient.addColorStop(0, "#3B82F6");
	gradient.addColorStop(0.5, "#8B5CF6");
	gradient.addColorStop(1, "#EC4899");

	ctx.fillStyle = gradient;
	ctx.fill();
	ctx.strokeStyle = "white";
	ctx.lineWidth = 3;
	ctx.stroke();

	ctx.restore();

	// 현재 표시 프레임 번호 (실제 업데이트된 프레임)
	const displayFrame =
		canvasStates.value[side].lastRenderedFrame >= 0
			? Math.floor(canvasStates.value[side].lastRenderedFrame / frameInterval)
			: 0;
	ctx.fillStyle = "#888";
	ctx.font = "14px Arial";
	ctx.textAlign = "center";
	ctx.fillText(`Rendered: ${displayFrame}`, centerX, height - 40);

	// 실제 마스터 프레임 번호
	ctx.fillStyle = "#666";
	ctx.font = "12px Arial";
	ctx.fillText(`Master: ${masterFrameNumber}`, centerX, height - 10);

	// 움직이는 점들 (부드러움 시각화)
	const numDots = 8;
	for (let i = 0; i < numDots; i++) {
		const angle = ((progress + i * 45) * Math.PI) / 180;
		const dotRadius = 120;
		const x = centerX + Math.cos(angle) * dotRadius;
		const y = centerY + Math.sin(angle) * dotRadius;

		ctx.beginPath();
		ctx.arc(x, y, 8, 0, Math.PI * 2);
		ctx.fillStyle = `hsl(${(progress + i * 30) % 360}, 70%, 60%)`;
		ctx.fill();
	}

	// FPS 표시
	const smoothnessColor =
		fps >= 60 ? "#22c55e" : fps >= 30 ? "#eab308" : "#ef4444";
	ctx.fillStyle = smoothnessColor;
	ctx.fillRect(10, 15, 60, 20);
	ctx.fillStyle = "white";
	ctx.font = "12px Arial";
	ctx.textAlign = "left";
	ctx.fillText(`${fps}fps`, 25, 28);
};

const animate = () => {
	if (!isPlaying.value) return;

	frameCount.value++;

	// 마스터 프레임레이트(60fps)로 애니메이션
	if (leftCanvas.value) {
		drawFrame(leftCanvas.value, leftFps.value, frameCount.value, "left");
	}
	if (rightCanvas.value) {
		drawFrame(rightCanvas.value, rightFps.value, frameCount.value, "right");
	}

	animationId = requestAnimationFrame(animate);
};

const startAnimations = () => {
	if (isPlaying.value) {
		animate();
	}
};

const stopAnimations = () => {
	if (animationId) {
		cancelAnimationFrame(animationId);
	}
};

watch([leftFps, rightFps], () => {
	if (useVideoMode.value) {
		// 비디오 모드에서는 소스 변경 감지하여 동기화
		setTimeout(() => syncVideos(), 100);
	}
	// 캔버스 모드에서는 실시간 변경을 위해 별도 처리 없음
});

watch(useVideoMode, (newMode) => {
	if (newMode) {
		// 비디오 모드로 변경 시 초기 설정
		videosLoaded.value = { left: false, right: false };
	} else {
		// 캔버스 모드로 변경 시 애니메이션 시작
		if (isPlaying.value) {
			startAnimations();
		}
	}
});

onMounted(() => {
	if (!useVideoMode.value) {
		startAnimations();
	}
});

onUnmounted(() => {
	stopAnimations();
});
</script>

<style scoped>
/* Range Slider 스타일링 */
.slider {
	-webkit-appearance: none;
	appearance: none;
	background: #e5e7eb;
	outline: none;
	border-radius: 15px;
}

.slider::-webkit-slider-thumb {
	-webkit-appearance: none;
	appearance: none;
	height: 20px;
	width: 20px;
	border-radius: 50%;
	background: #3b82f6;
	cursor: pointer;
	border: 2px solid white;
	box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.slider::-moz-range-thumb {
	height: 20px;
	width: 20px;
	border-radius: 50%;
	background: #3b82f6;
	cursor: pointer;
	border: 2px solid white;
	box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.slider::-webkit-slider-track {
	height: 8px;
	background: #e5e7eb;
	border-radius: 15px;
}

.slider::-moz-range-track {
	height: 8px;
	background: #e5e7eb;
	border-radius: 15px;
}
</style>
