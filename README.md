# TP_Noyau_Temps_Reels

0 (Re)prise en main

0.1 Premiers pas

  1.  Où se situe le fichier main.c ?
      Le fichier main.c se situe dans le dossier Core->Src
      ![image](https://user-images.githubusercontent.com/125466579/230084485-75f504f9-b557-4707-b96b-78e90a42add7.png)
      
  2.  À quoi servent les commentaires indiquant BEGIN et END (appelons ça des balises) ?
      Les commentaires BEGIN et END permettent de délimiter les endroits où l'utilisateur peut écrire son code. Tout ce qui est écrit en dehors de ces balises n'est           pas compilé.
      
  3.  Quels sont les paramètres à passer à HAL_Delay et HAL_GPIO_WritePin ?    
      ![image](https://user-images.githubusercontent.com/125466579/230079394-39f86adf-b0ae-413e-822a-342cf8f99ee3.png)
      Comme indiqué dans l'image ci-dessus, la fonction HAL_Delay demande le paramètre "Delay" qui est de type uint32_t, cette variable défini le temps d'attente en           millisecondes.
      ![image](https://user-images.githubusercontent.com/125466579/230079856-a7c3c1e4-55b9-492e-9c6f-1e45a23bb3e8.png)
      Comme indiqué dans l'image ci-dessus, la fonction HAL_GPIO_WritePin demande les paramètres GPIOx,GPIO_Pin et PinState.
        - GPIOx est le port GPIO utilisé (par exemple GPIOA, GPIOB, GPIOC, etc.)
        - GPIO_Pin est le numéro de la broche d'E/S (de 0 à 15)
        - PinState est l'état de la broche d'E/S à écrire (GPIO_PIN_RESET pour une valeur logique de 0, GPIO_PIN_SET pour une valeur logique de 1)
        
  4.  Dans quel fichier les ports d’entrée/sorties sont-ils définis ?
      Les ports d’entrée/sorties sont définis dans le fichier stm32f7xx_hal_gpio.h :
      ![image](https://user-images.githubusercontent.com/125466579/230081914-bc463b8d-f642-466c-9fcb-2ac6f1dda681.png)
      
  5.  Écrivez un programme simple permettant de faire clignoter la LED.
      ![image](https://user-images.githubusercontent.com/125466579/230084374-3eadabfe-c572-44fe-a666-d6fe24c52650.png)

      ![image](https://user-images.githubusercontent.com/125466579/230084287-2884fd69-048f-430c-bde1-f26d9adc7fd1.png)
      
  6.  Modifiez le programme pour que la LED s’allume lorsque le bouton USER est appuyé. Le bouton est sur PI11.
      ![image](https://user-images.githubusercontent.com/125466579/230085232-f47f2d5c-2309-42fc-8a92-01464dadfcf5.png)

      ![image](https://user-images.githubusercontent.com/125466579/230085085-74e6df64-8f54-415e-9d67-03bd09b75be7.png)
      
1 FreeRTOS, tâches et sémaphores

1.1 Tâche simple

  1.  En quoi le paramètre TOTAL_HEAP_SIZE a-t-il de l’importance ?
      
