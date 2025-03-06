#
## 
```c
SET_BIT(RCC->APB2ENR, RCC_APB2ENR_TIM1EN);  // Enable TIM1 clock
    HAL_RCC_GetClockConfig(&clkconfig, &pFLatency);  // Get clock configuration
      /* Compute TIM1 clock */
      uwTimclock = 2*HAL_RCC_GetPCLK2Freq();
```