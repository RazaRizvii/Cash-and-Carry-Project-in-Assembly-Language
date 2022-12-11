;
;	 _______________________________________________________________Cash and Carry_______________________________________________________________________
;	|																																					 |
;	|	* Documnetation:																																 |
;	|     ^^^^^^^^^^^^^^																																 |
;	|		In this cash and carry we have three sections:																								 |
;	|		->	Grocery, Stationary and lastly Vegetable area.																							 |
;	|		->  Each section has three products which user can buy.																						 |
;	|																																					 |
;	|		Cash and Carry also have membership policy according to which if user is a member than get discount on items.								 |
;	|		We ask user If he is a member or not and in which section he wants to went in.																 |
;	|																																					 |
;	|		If user is not a membver then he can went in section of his choice and buy any product and as much he wants.								 |
;	|		If he is a member than he is asked to enter membership code which he is told at time of getting membership.									 |
;	|																																					 |
;	|		If code is correct then he can get discount on products.																					 |
;	|																																					 |
;	|		Lastly, when user wants to exit then enters 0 and bill is displayed																			 |
;	|																																					 |
;	|		On bill total amount user has to pay is displayed.																							 |
;	|		When user enters amount then cash returned is displayed and user gets greetings of thanks for using our cash and carry						 |
;	|																																					 |
;	|																																					 |
;	|________________________________________________________________Documentation_______________________________________________________________________|
;																			
;
INCLUDE Irvine32.inc
.data
;
;	 ___________________________________________________________________________________________________________________________________________________
;	|                                                                                                                                                   |
;	|	->Setting all strings or statemnents which has to b displayed on console															            |
;	|	->all names of variables are set according to their console screen on which they have to displayed or according to their functionality          |
;	|                                                                                                                                                   |
;	|___________________________________________________________________________________________________________________________________________________|
;
	mainPrompt BYTE "___________________________________________________Cash and Carry_______________________________________________________", 0

	inputMain SDWORD ?                                 

	;	These Variables has to be displayed on main screen
	;   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
	promptExit BYTE "Enter 0 to exit... :) ", 0
	promptMain1 BYTE "Sections Available: ", 0
	promptMain2 BYTE "1- Grocery Area. ", 0
	promptMain3 BYTE "2- Stationary Area. ", 0
	promptMain4 BYTE "3- Vegetable Area. ", 0
	promptMain5 BYTE "Enter Section serial you want to get in: ", 0
	promptMain6 BYTE "Enter 4 If you have Membership...", 0


	;	These have to be displayed on normal user screen without discount
	;   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
	prompGrocery1 BYTE "Items Available in Grocery are: ", 0
	prompGrocery2 BYTE "1- Bread..............................Rs 100\- ", 0
	prompGrocery3 BYTE "2- Milk...............................Rs 250\- ", 0
	prompGrocery4 BYTE "3- Egg(Dozen).........................Rs 150\- ", 0
	prompGrocery5 BYTE "Enter item serial you want to buy: ", 0


	;	These have to be displayed on normal user screen without discount
	;   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
	prompStationary1 BYTE "Items Available in Stationary are: ", 0
	prompStationary2 BYTE "1- Register........................Rs 300\- ", 0
	prompStationary3 BYTE "2- geometry........................Rs 050\- ", 0
	prompStationary4 BYTE "3- Pen.............................Rs 010\- ", 0
	prompStationary5 BYTE "Enter item serial you want to buy: ", 0


	;	These have to be displayed on normal user screen without discount
	;   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
	prompVegetable1 BYTE "Items Available in Vegetables are: ", 0
	prompVegetable2 BYTE "1- Tomato(per Kilo).................Rs 150\- ", 0
	prompVegetable3 BYTE "2- Potato(per Kilo).................Rs 100\- ", 0
	prompVegetable4 BYTE "3- Onion(per Kilo)..................Rs 050\- ", 0
	prompVegetable5 BYTE "Enter item serial you want to buy: ", 0


	;	As according to their name they are used for billing system.
	;	  -> bill and returned are carriers for values of bill and other have to be displayed on console
	bill SDWORD ?
	returned SDWORD  ?
	prompBill1 BYTE "Bill: ", 0
	prompBill2 BYTE "Please Enter bill: ", 0
	prompBill3 BYTE "Cash Returned: ", 0
	prompBill4 BYTE "Thanks for shopping :) ", 


	;	These have to be displayed on user screen with discount
	;   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
	prompGroceryMember1 BYTE "Items Available in Grocery are: ", 0
	prompGroceryMember2 BYTE "1- Bread..............................Rs 070\- ", 0
	prompGroceryMember3 BYTE "2- Milk...............................Rs 200\- ", 0
	prompGroceryMember4 BYTE "3- Egg(Dozen).........................Rs 100\- ", 0
	prompGroceryMember5 BYTE "Enter item serial you want to buy: ", 0


	;	These have to be displayed on user screen with discount
	;   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
	prompStationaryMember1 BYTE "Items Available in Stationary are: ", 0
	prompStationaryMember2 BYTE "1- Register........................Rs 200\- ", 0
	prompStationaryMember3 BYTE "2- geometry........................Rs 025\- ", 0
	prompStationaryMember4 BYTE "3- Pen.............................Rs 010\- ", 0
	prompStationaryMember5 BYTE "Enter item serial you want to buy: ", 0


	;	These have to be displayed on user screen with discount
	;   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
	prompVegetableMember1 BYTE "Items Available in Vegetables are: ", 0
	prompVegetableMember2 BYTE "1- Tomato(per Kilo).................Rs 100\- ", 0
	prompVegetableMember3 BYTE "2- Potato(per Kilo).................Rs 080\- ", 0
	prompVegetableMember4 BYTE "3- Onion(per Kilo)..................Rs 030\- ", 0
	prompVegetableMember5 BYTE "Enter item serial you want to buy: ", 0

	passwordPrompt BYTE "Enter Membership password: ", 0


.code
main PROC
;
;	 _____________________________________________________________________________________
;	|																					  |
;	|	* It is based on If else or While loop.											  |
;	|	* First control enters loop and ask user for input.								  |
;	|	* Control enters in section of users choice.									  |
;	|																					  |		
;	|	* Program is based on Procedures.												  |
;	|	* Every function which is performed is encapsulated in functions.				  |
;	|																					  |
;	|	* Loop iterates on value of ebx register.										  |
;	|	* If its value becomes 0 or less than 0 then it terminates else it iterates.	  |
;	|																					  |	
;	|	* Note:																			  |	
;	|		-> Comments which will be same for all lines so defined only once			  |	
;	|_____________________________________________________________________________________|
;	
;
;
	mov bill, 0
	mov returned, 0	
	mov ebx, 1

	.WHILE ebx > 0
		call mainMenu
		call Input				   ;	Calling Input Procedure
								   ;	^^^^^^^^^^^^^^^^^^^^^^^
		.IF inputMain == 1         ;	Enters if user with no discount wents in grocery
			call groceryMenu       ;	^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
			call Input			   ;
								   ;
			.IF inputMain == 1	   ;
				add bill, 100	   ;	Calculating Bill
				mov inputMain, 5   ;	^^^^^^^^^^^^^^^^
			.ENDIF				   ;	

			.IF inputMain == 2
				add bill, 250
				mov inputMain, 5
			.ENDIF

			.IF inputMain == 3
				add bill, 150
				mov inputMain, 5
			.ENDIF

			.IF inputMain == 0
				mov ebx, 0
			.ENDIF
		.ENDIF

		.IF inputMain == 2         ;	Enters if user with no discount wents in stationary
			call stationaryMenu    ;	^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
			call Input
		
			.IF inputMain == 1
				add bill, 300
				mov inputMain, 5
			.ENDIF

			.IF inputMain == 2
				add bill, 50
				mov inputMain, 5
			.ENDIF

			.IF inputMain == 3
				add bill, 10
				mov inputMain, 5
			.ENDIF

			.IF inputMain == 0
				mov ebx, 0
			.ENDIF
		.ENDIF

		.IF inputMain == 3         ;	Enters if user with no discount wents in vegetable
			call vegetableMenu     ;	^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
			call Input
		
			.IF inputMain == 1
				add bill, 150
				mov inputMain, 5
			.ENDIF

			.IF inputMain == 2
				add bill, 100
				mov inputMain, 5
			.ENDIF

			.IF inputMain == 3
				add bill, 50
				mov inputMain, 5
			.ENDIF

			.IF inputMain == 0
				mov ebx, 0
			.ENDIF
		.ENDIF

		.IF inputMain == 4                   ;	Enters if user is a member and ask for password
			call passwordPromptMember        ;	^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
											 ;
			.IF inputMain == 1234            ;	Enters if password is correct
				call mainMenuMember			 ;	^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
				call Input

				.IF inputMain == 1           
					call groceryMemberMenu
					call Input
		
					.IF inputMain == 1
						add bill, 070
						mov inputMain, 5
					.ENDIF

					.IF inputMain == 2
						add bill, 200
						mov inputMain, 5
					.ENDIF

					.IF inputMain == 3
						add bill, 100
						mov inputMain, 5
					.ENDIF

					.IF inputMain == 0
						mov ebx, 0
					.ENDIF
				.ENDIF

				.IF inputMain == 2
					call stationaryMemberMenu
					call Input
		
					.IF inputMain == 1
						add bill, 200
						mov inputMain, 5
					.ENDIF

					.IF inputMain == 2
						add bill, 25
						mov inputMain, 5
					.ENDIF

					.IF inputMain == 3
						add bill, 10
						mov inputMain, 5
					.ENDIF

					.IF inputMain == 0
						mov ebx, 0
					.ENDIF
				.ENDIF

				.IF inputMain == 3
					call vegetableMemberMenu
					call Input
		
					.IF inputMain == 1
						add bill, 100
						mov inputMain, 5
					.ENDIF

					.IF inputMain == 2
						add bill, 80
						mov inputMain, 5
					.ENDIF

					.IF inputMain == 3
						add bill, 30
						mov inputMain, 5
					.ENDIF

					.IF inputMain == 0
						mov ebx, 0
					.ENDIF
				.ENDIF
			.ENDIF

			.IF inputMain == 0               ;	Enters if user wants to exit and enters 0
				mov ebx, 0					 ;	^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
			.ENDIF
		.ENDIF
		
		.IF inputMain == 0
			mov ebx, 0
		.ENDIF
	.ENDW

	call displayBill						 ;	Calling Bill procedure to display bill and perform all calculations
	call WaitMsg							 ;	^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
main ENDP
;
;	 _______________________________________________________________________________________
;	|																						|
;	|....As already told program is procedure based So all procedures are defined below.	|
;	|....Procedures names are set according to their functionality.							|
;	|_______________________________________________________________________________________|		
;
;
;
;....Procedure to Input Password 
;	 ^^^^^^^^^^^^^^^^^^^^^^^^^^^
passwordPromptMember PROC               ;	 _______________________________________________
	call CLRSCR							;	|	Calling function to clear previous screen	|
										;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^	|
	mov eax,yellow + (brown * 32)       ;	|	Setting colour of console					|
	call SetTextColor                   ;	|	^^^^^^^^^^^^^^^^^^^^^^^^^					|
	                                    ;	|												|
	mov edx , OFFSET mainPrompt         ;	|	Displaying message to user					|
	call WriteString                    ;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^					|
	call CRLF						    ;	|_______________________________________________|
	Call CRLF
	call CRLF
	Call CRLF

	mov edx , OFFSET passwordPrompt     
	call WriteString
	call CRLF
	Call CRLF

	call Input
	ret
passwordPromptMember ENDP


;....Procedure for Getting input from user
;	 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Input PROC							    ;	 _______________________________________________
	pushad								;	|	Function to get input from the user			|
	call ReadInt						;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^			|
	mov inputMain,eax					;	|	Uses built in function call ReadInt			|
	popad								;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^			|
										;	|	Moves input from eax to variable			|
	ret									;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^			|
Input ENDP								;	|_______________________________________________|


;....Procedure for Displaying bill
;	 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
displayBill PROC						;	 _______________________________________________
	call CLRSCR							;	|	Calling function to clear previous screen	|
										;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^	|
	mov eax,yellow + (brown * 39)       ;	|	Setting colour of console					|
	call SetTextColor                   ;	|	^^^^^^^^^^^^^^^^^^^^^^^^^					|
										;	|												|
	mov edx , OFFSET mainPrompt         ;	|	Displaying message to user					|
	call WriteString                    ;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^					|
	call CRLF						    ;	|_______________________________________________|
	Call CRLF
	call CRLF
	Call CRLF

	mov edx , OFFSET prompBill1
	call WriteString
	call CRLF
	Call CRLF

	mov eax, bill
	call WriteInt
	call CRLF
	
	mov edx , OFFSET prompBill2
	call WriteString
	call CRLF
	Call CRLF
	
	call Input
	mov eax, inputMain
	sub eax, bill
	mov returned, eax

	mov edx , OFFSET prompBill3
	call WriteString
	call CRLF
	Call CRLF

	mov eax, returned
	call WriteInt
	call CRLF

	mov edx , OFFSET prompBill4
	call WriteString
	call CRLF
	Call CRLF
	
	ret
displayBill ENDP


;....Procedure for Membership Main Menu
;	 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
mainMenuMember PROC					   ;	 _______________________________________________
	mov eax,yellow + (red * 12)        ;	|	Setting colour of console					|
	call SetTextColor                  ;	|	^^^^^^^^^^^^^^^^^^^^^^^^^					|
									   ;	|												|
	mov edx , OFFSET mainPrompt		   ;	|	Displaying message to user					|
	call WriteString				   ;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^					|
	call CRLF						   ;	|												|
	Call CRLF						   ;	|												|
	call CRLF						   ;	|												|
	Call CRLF						   ;	|												|
									   ;	|												|
	call CLRSCR						   ;	|	Calling function to clear previous screen	|
									   ;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^	|
	mov edx , OFFSET promptMain1	   ;	|_______________________________________________|
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET promptMain2
	call WriteString
	call CRLF
	Call CRLF

	mov edx , OFFSET promptMain3
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET promptMain4
	call WriteString
	call CRLF
	Call CRLF

	mov edx , OFFSET promptMain5
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET promptExit
	call WriteString
	call CRLF
	Call CRLF

	ret
mainMenuMember ENDP


;....Procedure for Main Menu
;	 ^^^^^^^^^^^^^^^^^^^^^^^
mainMenu PROC							;	 _______________________________________________
	mov eax,yellow + (red * 12)			;	|	Setting colour of console					|
	call SetTextColor                   ;	|	^^^^^^^^^^^^^^^^^^^^^^^^^					|
										;	|												|
	call CLRSCR							;	|	Calling function to clear previous screen	|
										;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^	|
	mov edx , OFFSET mainPrompt			;	|	Displaying message to user					|
	call WriteString					;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^					|
	call CRLF							;	|_______________________________________________|
	Call CRLF
	call CRLF
	Call CRLF

	mov edx , OFFSET promptMain1
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET promptMain2
	call WriteString
	call CRLF
	Call CRLF

	mov edx , OFFSET promptMain3
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET promptMain4
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET promptMain6
	call WriteString
	call CRLF
	Call CRLF

	mov edx , OFFSET promptMain5
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET promptExit
	call WriteString
	call CRLF
	Call CRLF

	ret
mainMenu ENDP


;....Procedure for Grocery Menu
;	 ^^^^^^^^^^^^^^^^^^^^^^^^^^
groceryMenu PROC						;	 _______________________________________________
	call CLRSCR							;	|	Calling function to clear previous screen	|
										;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^	|
	mov eax,yellow + (red * 25)         ;	|	Setting colour of console					|
	call SetTextColor                   ;	|	^^^^^^^^^^^^^^^^^^^^^^^^^					|
										;	|												|
	mov edx , OFFSET mainPrompt			;	|	Displaying message to user					|
	call WriteString					;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^					|
	call CRLF							;	|_______________________________________________|
	Call CRLF
	call CRLF
	Call CRLF

	mov edx , OFFSET prompGrocery1
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET prompGrocery2
	call WriteString
	call CRLF
	Call CRLF

	mov edx , OFFSET prompGrocery3
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET prompGrocery4
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET prompGrocery5
	call WriteString
	call CRLF
	Call CRLF

	mov edx , OFFSET promptExit
	call WriteString
	call CRLF
	Call CRLF

	ret
groceryMenu ENDP


;....Procedure for Stationary Menu
;	 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
stationaryMenu PROC						;	 _______________________________________________
	call CLRSCR							;	|	Calling function to clear previous screen	|
										;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^	|
	mov eax,yellow + (red * 25)         ;	|	Setting colour of console					|
	call SetTextColor                   ;	|	^^^^^^^^^^^^^^^^^^^^^^^^^					|
										;	|												|
	mov edx , OFFSET mainPrompt			;	|	Displaying message to user					|
	call WriteString					;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^					|
	call CRLF							;	|_______________________________________________|
	Call CRLF
	call CRLF
	Call CRLF

	mov edx , OFFSET prompStationary1
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET prompStationary2
	call WriteString
	call CRLF
	Call CRLF

	mov edx , OFFSET prompStationary3
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET prompStationary4
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET prompStationary5
	call WriteString
	call CRLF
	Call CRLF

	mov edx , OFFSET promptExit
	call WriteString
	call CRLF
	Call CRLF

	ret
stationaryMenu ENDP


;....Procedure for Vegetable Menu
;	 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
vegetableMenu PROC						;	 _______________________________________________
	call CLRSCR							;	|	Calling function to clear previous screen	|
										;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^	|
	mov eax,yellow + (red * 25)         ;	|	Setting colour of console					|
	call SetTextColor                   ;	|	^^^^^^^^^^^^^^^^^^^^^^^^^					|
										;	|												|
	mov edx , OFFSET mainPrompt			;	|	Displaying message to user					|
	call WriteString					;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^					|
	call CRLF							;	|_______________________________________________|
	Call CRLF
	call CRLF
	Call CRLF

	mov edx , OFFSET prompVegetable1
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET prompVegetable2
	call WriteString
	call CRLF
	Call CRLF

	mov edx , OFFSET prompVegetable3
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET prompVegetable4
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET prompVegetable5
	call WriteString
	call CRLF
	Call CRLF

	mov edx , OFFSET promptExit
	call WriteString
	call CRLF
	Call CRLF

	ret
vegetableMenu ENDP


;....Procedure for Grocery Member Menu
;	 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
groceryMemberMenu PROC					;	 _______________________________________________
	call CLRSCR							;	|	Calling function to clear previous screen	|
										;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^	|
	mov eax,yellow + (red * 25)         ;	|	Setting colour of console					|
	call SetTextColor                   ;	|	^^^^^^^^^^^^^^^^^^^^^^^^^					|
										;	|												|
	mov edx , OFFSET mainPrompt			;	|	Displaying message to user					|
	call WriteString					;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^					|
	call CRLF							;	|_______________________________________________|
	Call CRLF
	call CRLF
	Call CRLF

	mov edx , OFFSET prompGroceryMember1
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET prompGroceryMember2
	call WriteString
	call CRLF
	Call CRLF

	mov edx , OFFSET prompGroceryMember3
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET prompGroceryMember4
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET prompGroceryMember5
	call WriteString
	call CRLF
	Call CRLF

	mov edx , OFFSET promptExit
	call WriteString
	call CRLF
	Call CRLF

	ret
groceryMemberMenu ENDP


;....Procedure for Stationary Member Menu
;	 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
stationaryMemberMenu PROC				;	 _______________________________________________
	call CLRSCR							;	|	Calling function to clear previous screen	|
										;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^	|
	mov eax,yellow + (red * 25)         ;	|	Setting colour of console					|
	call SetTextColor                   ;	|	^^^^^^^^^^^^^^^^^^^^^^^^^					|
										;	|												|
	mov edx , OFFSET mainPrompt			;	|	Displaying message to user					|
	call WriteString					;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^					|
	call CRLF							;	|_______________________________________________|
	Call CRLF
	call CRLF
	Call CRLF

	mov edx , OFFSET prompStationaryMember1
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET prompStationaryMember2
	call WriteString
	call CRLF
	Call CRLF

	mov edx , OFFSET prompStationaryMember3
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET prompStationaryMember4
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET prompStationaryMember5
	call WriteString
	call CRLF
	Call CRLF

	mov edx , OFFSET promptExit
	call WriteString
	call CRLF
	Call CRLF

	ret
stationaryMemberMenu ENDP


;....Procedure for Vegetable Member Menu
;	 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
vegetableMemberMenu PROC				;	 _______________________________________________
	call CLRSCR							;	|	Calling function to clear previous screen	|
										;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^	|
	mov eax,yellow + (red * 25)         ;	|	Setting colour of console					|
	call SetTextColor                   ;	|	^^^^^^^^^^^^^^^^^^^^^^^^^					|
										;	|												|
	mov edx , OFFSET mainPrompt			;	|	Displaying message to user					|
	call WriteString					;	|	^^^^^^^^^^^^^^^^^^^^^^^^^^					|
	call CRLF							;	|_______________________________________________|
	Call CRLF
	call CRLF
	Call CRLF

	mov edx , OFFSET prompVegetableMember1
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET prompVegetableMember2
	call WriteString
	call CRLF
	Call CRLF

	mov edx , OFFSET prompVegetableMember3
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET prompVegetableMember4
	call WriteString
	call CRLF
	Call CRLF
	
	mov edx , OFFSET prompVegetableMember5
	call WriteString
	call CRLF
	Call CRLF

	mov edx , OFFSET promptExit
	call WriteString
	call CRLF
	Call CRLF

	ret
vegetableMemberMenu ENDP
END main
;																						 ______________
;																						|      :)      |
;_______________________________________________________________________________________|End Of Program|___________________________________________________________________________________________________
;                                                                                       |______________|
; 