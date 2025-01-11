# Canvas API 심화
> #### Canvas API란? ☞ [기본사용법(MDN Canvas API 문서)](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Basic_usage)
웹 브라우저에서 그림을 그릴 수 있게 해주는 도구(HTML의 <canvas> 태그와 JavaScript를 사용해서 다양한 그래픽 작업)
+ 2D 그래픽에 특화
+ 픽셀 단위로 세밀한 제어가 가능
+ 기본 크기는 300x150 픽셀이며, 원하는 크기로 조절할 수 있다.

## Canvas API로 데이터 시각화가 가능한 이유
1. **픽셀 기반 렌더링**
+ 픽셀 단위로 직접 그래픽을 조작할 수 있어 세밀한 제어가 가능
+ DOM 요소를 사용하지 않고 직접 그래픽을 그리므로 대량의 데이터 처리에 효율적 <br>
2. **실시간 성능**
+ 프레임 기반 렌더링으로 부드러운 애니메이션과 실시간 업데이트가 가능
+ 배치 업데이트를 통해 여러 변경사항을 한 번에 처리할 수 있어 성능이 우수 <br>
3. **메모리 효율성**
+ DOM 조작 없이 즉시 화면에 그릴 수 있어 메모리 사용이 효율적
+ 오프스크린 캔버스를 활용해 복잡한 연산을 백그라운드에서 처리 <br>
4. **유연한 상호작용**
+ 마우스와 터치 이벤트를 직접 처리할 수 있어 다양한 인터랙션 구현이 가능
+ 실시간으로 그래픽을 수정하고 업데이트할 수 있어 동적인 데이터 시각화에 적합 <br>

> #### 프로젝트를 간단히 진행 ☞ [canvas-api-example 리포지토리](https:1//github.com/hyungoo7703/canvas-api-example)
Canvas API는 최근까지도 데이터 시각화에 활발하게 이용되기에 학습의 필요성이 있다고 판단했다. <br>
실제로 코드를 작성을 진행하고, 정리를 진행할 예정

### Canvas API 시각화 처리

> #### 차트로 처리
+ 실시간 데이터 업데이트와 애니메이션 구현이 용이
```js
function drawChart() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    
    const chartWidth = canvas.width - (CHART_CONFIG.startX + CHART_CONFIG.rightMargin);
    const chartHeight = canvas.height - (CHART_CONFIG.startY + CHART_CONFIG.bottomMargin);
    
    // 축 그리기를 먼저 수행
    drawAxes();
    
    chartState.data.forEach((value, index) => {
        const x = CHART_CONFIG.startX + (CHART_CONFIG.barWidth + CHART_CONFIG.spacing) * index;
        const height = (value / Math.max(...chartState.data)) * chartHeight;
        const y = canvas.height - CHART_CONFIG.bottomMargin - height;
        
        // 막대 그리기
        ctx.fillStyle = '#4CAF50';
        ctx.fillRect(x, y, CHART_CONFIG.barWidth, height);
        
        // 값 표시
        ctx.fillStyle = '#000';
        ctx.font = '14px Arial';
        ctx.textAlign = 'center';
        ctx.fillText(value, x + CHART_CONFIG.barWidth/2, y - 10);
        
        // x축 레이블
        ctx.fillText(index + 1, x + CHART_CONFIG.barWidth/2, canvas.height - CHART_CONFIG.bottomMargin + 20);
    });
}
```

> #### 실시간 데이터 처리
+ 실시간 정보를 동적으로 표현
+ setInterval이나 requestAnimationFrame을 사용하여 지속적인 데이터 업데이트
```js
function startAutoUpdate() {
    function update() {
        if (chartState.isAutoUpdating) {
            // 랜덤하게 하나의 데이터 업데이트
            const randomIndex = Math.floor(Math.random() * chartState.data.length);
            chartState.data[randomIndex] = Math.floor(Math.random() * 100);
            drawChart();
            requestAnimationFrame(update);
        }
    }
    requestAnimationFrame(update);
}
```

> #### requestAnimationFrame
requestAnimationFrame(rAF)은 브라우저에서 애니메이션을 최적화하여 구현할 수 있게 해주는 Web API

작동방식
+ 브라우저의 리페인트(repaint) 주기에 맞춰 애니메이션을 실행
+ 일반적으로 1초에 60프레임(60fps)으로 동작
+ 모니터의 주사율에 맞춰 자동으로 최적화



