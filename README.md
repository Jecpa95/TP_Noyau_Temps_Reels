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
      Il détermine la quantité de mémoire disponible pour allouer des tâches et des ressources dans le système. Cette mémoire est exclusivement réservé pour freeRTOS
      Dans le fichier FreeRTOSConfig.h, on retrouve la configuration faites, comme on peut le voir pour le paramètre TOTAL_HEAP_SIZE : 
      ![image](https://user-images.githubusercontent.com/125466579/236872011-932165c9-fe78-41a9-8192-912695cd9c9c.png)
      
  2. Créez une tâche permettant de faire changer l’état de la LED toutes les 100ms et profitez-en pour afficher du texte à chaque changement d’état.
     ![image](https://user-images.githubusercontent.com/125466579/236872752-21147bd6-b980-42ff-848c-60f8c7d04d54.png)
    
     Quel est le rôle de la macro portTICK_PERIOD_MS ?
     La macro portTICK_PERIOD_MS est utilisée dans FreeRTOS pour convertir un temps en millisecondes en une valeur de temps d'attente qui peut être utilisée dans les fonctions de temporisation de FreeRTOS. par exemple vTaskDelay.
     
1.2 Sémaphores pour la synchronisation
  
  Création de deux tâches, taskGive et taskTake, ayant deux priorités differentes. TaskGive donne un sémaphore toutes les 100ms. Les 2 taches affichent du texte avant et après avoir donné le sémaphore. TaskTake prend le sémaphore. il y a une gestion d’erreur lors de l’acquisition du sémaphore. on invoque un reset software au STM32 si le sémaphore n’est pas acquis au bout d’une seconde. Pour valider la gestion d’erreur, on ajoute 100ms au delai de TaskGive à chaque itération :
  ![image](https://user-images.githubusercontent.com/125466579/236873880-506a1d8a-0b21-4fa5-9bef-1c8b99640ed1.png)
  
  Premier test avec TaskTake priorité 1 et TaskGive priorité 2 : 
  ![image](https://user-images.githubusercontent.com/125466579/236874431-295041d6-b1dc-4c63-b881-c916007df59b.png)
  ![image](https://user-images.githubusercontent.com/125466579/236874443-6f28d449-0ce4-44f6-b9bf-ee08ba313f71.png)

  Deuxième test avec TaskTake priorité 2 et TaskGive priorité 1 : 
  ![image](https://user-images.githubusercontent.com/125466579/236874512-ce5f1529-afd3-4ccc-81b6-be4050d0fa2c.png)
  ![image](https://user-images.githubusercontent.com/125466579/236874536-9a764549-be55-4505-b2f4-17da01aa2e5a.png)

  Dans le deuxième test TaskTake a une priorité plus élevé, donc elle est appelé en priorité. De ce fait TaskTake a le temps de réaliser tout son processus avant que TaskGive ne soit appelé. à l'invers du premier test.
  
1.3 Notification
  
  ![image](https://user-images.githubusercontent.com/125466579/236875848-eb6c5ce4-ba2b-4460-8197-94630aae2483.png)
  
  Premier test :
  ![image](https://user-images.githubusercontent.com/125466579/236875561-7155c30a-30f2-4016-b2be-ba4d2fbb66ad.png)
  ![image](https://user-images.githubusercontent.com/125466579/236875576-a6ea798f-8a85-4381-8032-0b92d1d657cc.png)

  Deuxième test : 
  ![image](https://user-images.githubusercontent.com/125466579/236875910-2b29a04e-d084-49a9-a360-2c5c10243e65.png)
  ![image](https://user-images.githubusercontent.com/125466579/236875928-442ece3f-ed10-4dd6-9598-93e75847ec1d.png)

1.4 Queues
  
  ![image](https://user-images.githubusercontent.com/125466579/236876070-2d604333-1069-4f34-af66-44b7652f9d9b.png)
  ![image](https://user-images.githubusercontent.com/125466579/236876153-edbb175f-f9ee-4983-a2d9-6f91be951e1f.png)

1.5 Réentrance et exclusion mutuelle
  
   Problème du code : dans le printf la valeur de « delay » est toujours à 2. Le printf n’a pas le temps de finir qu’on passe directement dans l’autre tâche, donc le      printf termine son buffer avec le deuxième printf.
   ![image](https://user-images.githubusercontent.com/125466579/236876930-ae904f76-0f48-43b2-bbef-c1e1ce5ceb3f.png)


  Pour résoudre ce problème on pourrait utiliser des Sémaphores mutex :
  Il faut au préalable activer la réentrance : 
  ![image](https://user-images.githubusercontent.com/125466579/236877109-d5a0a6f7-6d2c-49e3-851a-89358d315f9c.png)

  ![image](https://user-images.githubusercontent.com/125466579/236876799-991533f3-62b7-44c3-91e9-1fc733d1b577.png)
  ![image](https://user-images.githubusercontent.com/125466579/236876950-af4fff59-c184-4c21-b352-ef7620cbbc5d.png)


