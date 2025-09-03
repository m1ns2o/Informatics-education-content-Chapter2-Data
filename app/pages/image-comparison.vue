<template>
	<div class="container mx-auto px-4 py-8">
		<div class="max-w-6xl mx-auto">
			<div class="text-center mb-8">
				<h1 class="text-3xl font-bold text-gray-900 mb-4">
					벡터 vs 비트맵 이미지 비교
				</h1>
				<p class="text-lg text-gray-600">
					동일한 이미지를 벡터와 비트맵으로 비교하고 확대해서 차이점을
					확인해보세요
				</p>
			</div>

			<!-- 확대 레벨 컨트롤 -->
			<UCard class="mb-8">
				<template #header>
					<h3 class="text-xl font-semibold flex items-center gap-2">
						<UIcon name="i-lucide-zoom-in" class="w-5 h-5 text-blue-500" />
						확대 레벨 조정
					</h3>
				</template>

				<div class="space-y-4">
					<div class="flex items-center gap-4">
						<UButton
							@click="zoomOut"
							:disabled="zoomLevel <= 1"
							icon="i-lucide-zoom-out"
							variant="outline"
							size="sm"
						>
							축소
						</UButton>

						<UButton
							@click="zoomIn"
							:disabled="zoomLevel >= 8"
							icon="i-lucide-zoom-in"
							variant="outline"
							size="sm"
						>
							확대
						</UButton>

						<UBadge color="primary" variant="soft" size="lg">
							{{ zoomLevel }}x
						</UBadge>
					</div>

					<div class="space-y-3">
						<div class="flex items-center justify-between">
							<label class="text-sm font-medium text-gray-700">
								확대 레벨: {{ zoomLevel }}x
							</label>
							<div class="flex gap-1">
								<UButton
									@click="setZoomLevel(1)"
									variant="ghost"
									size="xs"
									:color="zoomLevel === 1 ? 'primary' : 'gray'"
								>
									1x
								</UButton>
								<UButton
									@click="setZoomLevel(2)"
									variant="ghost"
									size="xs"
									:color="zoomLevel === 2 ? 'primary' : 'gray'"
								>
									2x
								</UButton>
								<UButton
									@click="setZoomLevel(4)"
									variant="ghost"
									size="xs"
									:color="zoomLevel === 4 ? 'primary' : 'gray'"
								>
									4x
								</UButton>
								<UButton
									@click="setZoomLevel(8)"
									variant="ghost"
									size="xs"
									:color="zoomLevel === 8 ? 'primary' : 'gray'"
								>
									8x
								</UButton>
							</div>
						</div>

						<input
							type="range"
							v-model="zoomLevel"
							:min="1"
							:max="8"
							:step="0.25"
							class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer slider"
						/>

						<div class="flex justify-between text-xs text-gray-500 mt-1">
							<span>1x</span>
							<span>2.75x</span>
							<span>4.5x</span>
							<span>6.25x</span>
							<span>8x</span>
						</div>
					</div>
				</div>
			</UCard>

			<div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
				<!-- 벡터 이미지 -->
				<UCard>
					<template #header>
						<h3 class="text-xl font-semibold flex items-center gap-2">
							<UIcon name="i-lucide-vector" class="w-5 h-5 text-green-500" />
							벡터 이미지 (SVG)
						</h3>
					</template>

					<div class="space-y-4">
						<div
							class="border border-gray-200 rounded-lg overflow-hidden bg-white"
							:style="{ height: `${baseHeight}px` }"
						>
							<div
								class="flex items-center justify-center h-full overflow-hidden"
								ref="vectorContainer"
							>
								<div
									:style="{
										transform: `scale(${zoomLevel})`,
										transformOrigin: 'center',
									}"
								>
									<svg
										:width="baseSize"
										:height="baseSize"
										viewBox="0 0 200 200"
										class="border"
									>
										<!-- 간단한 로고/아이콘 SVG -->
										<circle cx="100" cy="70" r="30" fill="#3B82F6" />
										<polygon points="100,110 85,125 115,125" fill="#10B981" />
										<text
											x="100"
											y="150"
											text-anchor="middle"
											font-family="Arial, sans-serif"
											font-size="16"
											fill="#374151"
										>
											LOGO
										</text>
									</svg>
								</div>
							</div>
						</div>

						<div class="text-sm text-gray-600 space-y-2">
							<h4 class="font-semibold text-gray-800">벡터 이미지 특징:</h4>
							<ul class="list-disc list-inside space-y-1">
								<li>수학적 공식으로 정의된 도형</li>
								<li>무한 확대 가능 (해상도 독립적)</li>
								<li>선명한 경계선 유지</li>
								<li>작은 파일 크기 (단순한 도형의 경우)</li>
								<li>로고, 아이콘에 적합</li>
							</ul>
						</div>
					</div>
				</UCard>

				<!-- 비트맵 이미지 -->
				<UCard>
					<template #header>
						<h3 class="text-xl font-semibold flex items-center gap-2">
							<UIcon name="i-lucide-image" class="w-5 h-5 text-orange-500" />
							비트맵 이미지 (PNG)
						</h3>
					</template>

					<div class="space-y-4">
						<div
							class="border border-gray-200 rounded-lg overflow-hidden bg-white"
							:style="{ height: `${baseHeight}px` }"
						>
							<div
								class="flex items-center justify-center h-full overflow-hidden"
								ref="bitmapContainer"
							>
								<canvas
									ref="bitmapCanvas"
									class="border"
									:style="{
										width: `${baseSize}px`,
										height: `${baseSize}px`,
										transform: `scale(${zoomLevel})`,
										transformOrigin: 'center',
										imageRendering: 'pixelated',
									}"
								/>
							</div>
						</div>

						<div class="text-sm text-gray-600 space-y-2">
							<h4 class="font-semibold text-gray-800">비트맵 이미지 특징:</h4>
							<ul class="list-disc list-inside space-y-1">
								<li>픽셀(점)로 구성된 격자 구조</li>
								<li>확대 시 픽셀이 보임 (해상도 의존적)</li>
								<li>계단 현상 (Aliasing) 발생</li>
								<li>사진에 적합</li>
								<li>복잡한 이미지는 큰 파일 크기</li>
							</ul>
						</div>
					</div>
				</UCard>
			</div>

			<!-- 비교 분석 -->
			<UCard class="mt-8">
				<template #header>
					<h3 class="text-xl font-semibold flex items-center gap-2">
						<UIcon name="i-lucide-eye" class="w-5 h-5 text-purple-500" />
						현재 확대 레벨에서의 차이점
					</h3>
				</template>

				<div class="grid grid-cols-1 md:grid-cols-2 gap-6">
					<div>
						<h4 class="font-semibold text-green-600 mb-2">벡터 이미지</h4>
						<div v-if="zoomLevel <= 2" class="text-sm text-gray-600">
							• 선명하고 깨끗한 경계선<br />
							• 매끄러운 곡선<br />
							• 색상 전환이 부드러움
						</div>
						<div v-else-if="zoomLevel <= 4" class="text-sm text-gray-600">
							• 여전히 선명한 품질 유지<br />
							• 곡선이 매끄럽게 렌더링<br />
							• 확대해도 품질 저하 없음
						</div>
						<div v-else class="text-sm text-gray-600">
							• 극도로 확대해도 완벽한 품질<br />
							• 수학적 정확성 유지<br />
							• 무한 확대 가능의 증명
						</div>
					</div>

					<div>
						<h4 class="font-semibold text-orange-600 mb-2">비트맵 이미지</h4>
						<div v-if="zoomLevel <= 2" class="text-sm text-gray-600">
							• 적당한 품질 유지<br />
							• 미세한 픽셀 경계 시작<br />
							• 원본 해상도 내에서 양호
						</div>
						<div v-else-if="zoomLevel <= 4" class="text-sm text-gray-600">
							• 픽셀이 명확히 보이기 시작<br />
							• 경계선이 계단 모양으로 변함<br />
							• 이미지 품질 저하 시작
						</div>
						<div v-else class="text-sm text-gray-600">
							• 픽셀이 크게 확대되어 보임<br />
							• 심각한 품질 저하<br />
							• 계단 현상이 매우 뚜렷함
						</div>
					</div>
				</div>
			</UCard>

			<!-- 실용적인 사용 예시 -->
			<UCard class="mt-8">
				<template #header>
					<h3 class="text-xl font-semibold flex items-center gap-2">
						<UIcon name="i-lucide-lightbulb" class="w-5 h-5 text-yellow-500" />
						언제 어떤 형식을 사용할까요?
					</h3>
				</template>

				<div class="grid grid-cols-1 md:grid-cols-2 gap-8">
					<div>
						<h4 class="text-lg font-semibold text-green-600 mb-3">
							벡터 이미지 (SVG) 사용 시기
						</h4>
						<ul class="space-y-2 text-sm">
							<li class="flex items-start gap-2">
								<UIcon
									name="i-lucide-check"
									class="w-4 h-4 text-green-500 mt-0.5 flex-shrink-0"
								/>
								<span>로고, 아이콘, 브랜드 마크</span>
							</li>
							<li class="flex items-start gap-2">
								<UIcon
									name="i-lucide-check"
									class="w-4 h-4 text-green-500 mt-0.5 flex-shrink-0"
								/>
								<span>단순한 일러스트레이션</span>
							</li>
							<li class="flex items-start gap-2">
								<UIcon
									name="i-lucide-check"
									class="w-4 h-4 text-green-500 mt-0.5 flex-shrink-0"
								/>
								<span>다양한 크기로 사용해야 하는 그래픽</span>
							</li>
							<li class="flex items-start gap-2">
								<UIcon
									name="i-lucide-check"
									class="w-4 h-4 text-green-500 mt-0.5 flex-shrink-0"
								/>
								<span>프린트용 고해상도 출력물</span>
							</li>
							<li class="flex items-start gap-2">
								<UIcon
									name="i-lucide-check"
									class="w-4 h-4 text-green-500 mt-0.5 flex-shrink-0"
								/>
								<span>웹사이트의 확장 가능한 그래픽</span>
							</li>
						</ul>
					</div>

					<div>
						<h4 class="text-lg font-semibold text-orange-600 mb-3">
							비트맵 이미지 (PNG/JPG) 사용 시기
						</h4>
						<ul class="space-y-2 text-sm">
							<li class="flex items-start gap-2">
								<UIcon
									name="i-lucide-check"
									class="w-4 h-4 text-orange-500 mt-0.5 flex-shrink-0"
								/>
								<span>사진, 복잡한 이미지</span>
							</li>
							<li class="flex items-start gap-2">
								<UIcon
									name="i-lucide-check"
									class="w-4 h-4 text-orange-500 mt-0.5 flex-shrink-0"
								/>
								<span>그라데이션이 많은 이미지</span>
							</li>
							<li class="flex items-start gap-2">
								<UIcon
									name="i-lucide-check"
									class="w-4 h-4 text-orange-500 mt-0.5 flex-shrink-0"
								/>
								<span>디지털 아트, 페인팅</span>
							</li>
							<li class="flex items-start gap-2">
								<UIcon
									name="i-lucide-check"
									class="w-4 h-4 text-orange-500 mt-0.5 flex-shrink-0"
								/>
								<span>스크린샷, 화면 캡처</span>
							</li>
							<li class="flex items-start gap-2">
								<UIcon
									name="i-lucide-check"
									class="w-4 h-4 text-orange-500 mt-0.5 flex-shrink-0"
								/>
								<span>고정된 크기로만 사용하는 이미지</span>
							</li>
						</ul>
					</div>
				</div>
			</UCard>

			<PageNavigation
				:previous-page="{ path: '/golden-ear', label: '황금귀 테스트' }"
				:next-page="{
					path: '/framerate-comparison',
					label: '프레임레이트 비교',
				}"
			/>
		</div>
	</div>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
import PageNavigation from "../components/PageNavigation.vue";

const zoomLevel = ref(1);
const baseSize = ref(200);
const baseHeight = ref(300);
const bitmapCanvas = ref<HTMLCanvasElement | null>(null);

const zoomIn = () => {
	if (zoomLevel.value < 8) zoomLevel.value += 0.25;
};

const zoomOut = () => {
	if (zoomLevel.value > 1) zoomLevel.value -= 0.25;
};

const setZoomLevel = (level: number) => {
	zoomLevel.value = level;
};

const drawBitmap = () => {
	if (!bitmapCanvas.value) return;

	const canvas = bitmapCanvas.value;
	const ctx = canvas.getContext("2d");
	if (!ctx) return;

	// 실제 비트맵 효과를 위해 작은 해상도로 그리기
	const pixelSize = 200; // 200x200 픽셀로 그리기
	canvas.width = pixelSize;
	canvas.height = pixelSize;

	// 픽셀화 효과를 위해 스무딩 비활성화
	ctx.imageSmoothingEnabled = false;

	// 배경색
	ctx.fillStyle = "white";
	ctx.fillRect(0, 0, pixelSize, pixelSize);

	// 원 그리기 (파란색)
	ctx.fillStyle = "#3B82F6";
	ctx.beginPath();
	ctx.arc(100, 70, 30, 0, Math.PI * 2);
	ctx.fill();

	// 삼각형 그리기 (초록색)
	ctx.fillStyle = "#10B981";
	ctx.beginPath();
	ctx.moveTo(100, 110);
	ctx.lineTo(85, 125);
	ctx.lineTo(115, 125);
	ctx.closePath();
	ctx.fill();

	// 텍스트 그리기
	ctx.fillStyle = "#374151";
	ctx.font = "16px Arial";
	ctx.textAlign = "center";
	ctx.fillText("LOGO", 100, 150);
};

onMounted(() => {
	drawBitmap();
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
	background: linear-gradient(
		to right,
		#3b82f6 0%,
		#3b82f6 calc((var(--value) - 1) / 7 * 100%),
		#e5e7eb calc((var(--value) - 1) / 7 * 100%),
		#e5e7eb 100%
	);
	border-radius: 15px;
}

.slider::-moz-range-track {
	height: 8px;
	background: #e5e7eb;
	border-radius: 15px;
}
</style>
