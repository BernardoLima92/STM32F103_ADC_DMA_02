# STM32F103_ADC_DMA_02
Using ADC trigged by TIM4 Capture Compare 4 Event and DMA


Using ADC trigged by TIMER and DMA

Neste tutorial o ADC1 do STM32F103C8T6 (Bluepill) é acionado pelo Trigger do Timer 4. Todo o código foi desenvolvido no STMCubeIDE.
A Figura abaixo mostra como o programa foi pensado:

![Diagramatempo](https://user-images.githubusercontent.com/114233216/192276086-33fb1ae2-88d6-411f-80dc-d5cba55cc17d.png)

Quando o contador do Timer4 (TIM4_CNT) for igual a 513, que é o valor existente no registrador do canal 4 (TIM4_CCR4), um sinal de trigger "Toggle on Match" é produzido
e enviado para o ADC para que uma conversão seja iniciada via DMA. Obs: A conversão só é iniciada nas bordas de subida.

Note: When an external trigger is selected for ADC regular or injected conversion, only the rising edge of the signal can start the conversion. 
(11.7 Conversion on external trigger, Pág 225 from RM0008 Reference manual)


No TIMER4, o canal 4 foi configurado como "Output Compare no Output" no modo "Toggle on Match", e o Trigger Output (TRGO) foi configurada como
"Output Compare (OC4REF)"
No ADC1 a fonte de trigger foi configurada como "Timer 4 Capture Compare 4 Event".

As figuras abaixo mostram as configurações realizadas no STMCubeIDE:

Configuração do TIMER4:

![Tim4_1](https://user-images.githubusercontent.com/114233216/192274905-bb26a155-7c74-4d70-bbd1-07b087ffc9f5.png)


Configuração do ADC1:

![ADC1](https://user-images.githubusercontent.com/114233216/192274942-8825adfa-fdfb-4610-aaab-2da7965ecefd.png)

Configuração do DMA

![DMA_adc](https://user-images.githubusercontent.com/114233216/192274983-8ccfef9f-a7ee-4534-b1fb-aa6c3660b9d1.png)

Parte Principal do Código:
![codigo pt1](https://user-images.githubusercontent.com/114233216/192275452-8c763004-d4a0-4d7a-92a9-be85da0b8639.png)

![codigo pt3](https://user-images.githubusercontent.com/114233216/192275498-94841f6e-ada0-4eec-8fcc-a1b2f2f478f4.png)

![codigo pt4](https://user-images.githubusercontent.com/114233216/192275514-94643289-171d-4081-ba8b-1972913d8da5.png)


Resultado do Experimento

![3](https://user-images.githubusercontent.com/114233216/192277039-88636c90-cf1e-4e16-bf05-add2ad961c6d.png)

Conectei um osciloscópio à porta PB7. O resultado está mostrado na figura acima. Tanto a borda superior quanto a borda inferior desse sinal amarelo
indicam o inicio do preenchimento do buffer com o resultado das conversões ADC. Como pode ser visto, para preencher o buffer com 18 conversões ADC 
foram necessários aproximadamente 3,4mS. 

O Clock do Timer4 é de 8MHz, logo o contador do timer incrementa a cada 125nS. Um ciclo completo (768 pulsos de clock) dura 96uS.
Assim, a cada 96uS uma conversão ADC é realizada. Como são realizadas 18 conversões no total, temos que 18 * 96uS = 1,72mS 
Porém, como o trigger só ocorre nas bordas de subida, a conta correta é 18 * 192uS = 3,45mS.
A figura abaixo ilustra esse raciocínio.

![triggertime](https://user-images.githubusercontent.com/114233216/192282216-11a51706-4533-49fa-8f9e-a9a8790451ef.png)

