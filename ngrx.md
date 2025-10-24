# GEMME BRUGER FORLØB i NgRx

### Parent side: 
**USER-CONTROL-PAGE:** 
- (click)="openNewUserModal()" => this.showNewUserModal = true; -->

### Child side: 
**NEW-USER-MODAL:**
- (ngSubmit)="onSaveUser() ==>   onSaveUser() {
	this.errorMessage = null; 
	this.store.dispatch(UserActions.addUser({ user: this.newUser }));
	  }
  
Den action der skal håndteres er defineret i UserActions

```
export const addUser = createAction('[Users] Add User', props<{ user: CreateUserDto }>());
```


--------------------------------------------------------------------------------
	this.store.dispatch(UserActions.addUser({ user: this.newUser }));  
--------------------------------------------------------------------------------

this.store 
----------
er en instans af NgRx’s Store-klasse, som du typisk får via dependency injection
Store fungerer som en central databank for hele Angular-applikationen.

this.store dispatch
-------------------
Metoden dispatch() betyder “send en besked om, at noget skal ske”
Der er en Action, der skal håndteres

UserActions.addUser
-------------------
Reference til en action creator, defineret i user.actions.ts
--> 	export const addUser = createAction('[Users] Add User', props<{ user: CreateUserDto }>());

{ user: this.newUser }
----------------------
payloaden du sender med actionen
this.newUser: et objekt
this.newUser = { firstName: 'Morten', lastName: 'Hansen', loginName: 'mh', role: 'Admin' };

Kaldet UserActions.addUser({ user: this.newUser }), skaber NgRx en action-instans:
{
  type: '[User] Add User',
  user: { firstName: 'Morten', lastName: 'Hansen', loginName: 'mh', role: 'Admin' }
}


--------------------------------------------------------------------------------------------------
 export const addUser = createAction('[Users] Add User', props<{ user: CreateUserDto }>()); 
--------------------------------------------------------------------------------------------------

createAction()				Opretter en ny NgRx action
'[Users] Add User'			Etiket, der beskriver hvad actionen gør
props<{ user: CreateUserDto }>()	Definerer payloadens type
{ user: this.newUser }			Objekt du sender, som matcher props-definitionen
Tuborg-klammer {}			Er nødvendige, fordi du sender et objekt med property user



export const addUser
--------------------
— eksport af en action creator (en funktion) med navnet addUser

createAction('[Users] Add User', ...)
--------------------------------------
- NgRx-funktion der opretter en action creator. 
	Den første parameter er en type-string ('[Users] Add User') 
	som bruges til logging/DevTools og til at skelne actions.	
	

**Dette er, hvad der returneres:**
```
{
  type: '[User] Add User',
  user: { firstName: 'Morten', lastName: 'Hansen', loginName: 'mh', role: 'Admin' }
}
```

| Trin		| 	Hvad sker der																|	Action								|	Hvem står for det	|	Hvem står for det
| 1️⃣		| 	Du fortæller systemet, at du vil oprette en bruger							|	addUser								| 	Component 			|
| 2️⃣		|	Effect fanger addUser, kalder API’et og venter på svar					 	|	(ingen ny action endnu)				|	Effect 				|
| 3️⃣		|	Når API’et svarer (enten succes eller fejl), sender effect en ny action:	|	addUserSuccess eller addUserFailure	| Effect






NgRx
Action → beskriver hvad der skal ske

Reducer → bestemmer hvordan staten ændres

Store → holder på den globale state
