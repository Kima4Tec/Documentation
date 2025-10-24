# GEMME BRUGER FORLØB i NgRx


## NgRx
- Action → beskriver hvad der skal ske

- Reducer → bestemmer hvordan staten ændres

- Store → holder på den globale state

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

 |	createAction()						|	Opretter en ny NgRx action 									|
 |	---------------						|	--------------------------									|
 |	'[Users] Add User'					|	Etiket, der beskriver hvad actionen gør						|
 |	props<{ user: CreateUserDto }>()	|	Definerer payloadens type									|
 |	{ user: this.newUser }				| 	Objekt du sender, som matcher props-definitionen			|
 |	Tuborg-klammer {}					|	Er nødvendige, fordi du sender et objekt med property user	|



## export const addUser
--------------------
eksport af en action creator (en funktion) med navnet addUser

## createAction('[Users] Add User', ...)
--------------------------------------
NgRx-funktion der opretter en action creator. 
	Den første parameter er en type-string ('[Users] Add User') 
	som bruges til logging/DevTools og til at skelne actions.	
	

**Dette er, hvad der returneres:**
```
{
  type: '[User] Add User',
  user: { firstName: 'Morten', lastName: 'Hansen', loginName: 'mh', role: 'Admin' }
}
```

| Trin		| 	Hvad sker der																|	Action								|	Hvem står for det	|
| -----		| 	-------------																|	------								|	-----------------	|
| 1️⃣		| 	Du fortæller systemet, at du vil oprette en bruger							|	addUser								| 	Component 			|
| 2️⃣		|	Effect fanger addUser, kalder API’et og venter på svar					 	|	(ingen ny action endnu)				|	Effect 				|
| 3️⃣		|	Når API’et svarer (enten succes eller fejl), sender effect en ny action:	|	addUserSuccess eller addUserFailure	| 	Effect				|



---
## Effects

```angular
export const addUserEffect = createEffect(
  (actions$ = inject(Actions), api = inject(UserApiService)) => {
    return actions$.pipe(
      ofType(UserActions.addUser),
      mergeMap(({ user }: { user: CreateUserDto }) =>
        api.addUser(user).pipe(
          map((createdUser: User) =>
            UserActions.addUserSuccess({ user: createdUser })
          ),
          catchError(error =>
            of(UserActions.addUserFailure({ error: error.message }))
          )
        )
      )
    );
  },
  { functional: true }
);

```
#### Et Effect i NgRx er:

- En observer, der lytter på Actions-streamen.
- Kan udføre side-effekter (API-kald, logning, navigation osv.).
- Kan dispatch’e nye actions baseret på resultatet (success/failure).

**createEffect** bruges til at definere effekten.

```functional: true```

- Dette gør det til en funktionel effect i stedet for en class-baseret.
- Du behøver ikke en effect-klasse med constructor(private actions$: Actions).
- I stedet bruges inject() direkte inde i effekten.

```inject(Actions) og inject(UserApiService)```
- inject() er Angular 17+ måde at hente services uden constructor.

```actions$.pipe(ofType(UserActions.addUser))```
- actions$ er alle actions i appen.
- ofType(UserActions.addUser) filtrerer kun de actions, der matcher addUser.
- Når du dispatch’er addUser fra komponenten, vil denne effekt “fange” den.
- pipe() er en RxJS-metode, der bruges til at kæde “operators” sammen på en Observable. Den tager input-streamen (actions$) og sender den gennem én eller flere operators, som fx: map, filter, mergeMap, catchError osv.

```mergeMap(({ user }: { user: CreateUserDto }) => ...)```
- Når en addUser action kommer ind, destructurerer vi payloaden { user }.
- TypeScript: user: CreateUserDto.
- mergeMap bruges fordi API-kaldet er asynkront (returnerer Observable<User>).
- mergeMap gør det muligt at håndtere flere addUser-actions samtidigt, hvis brugeren klikker flere gange hurtigt.

```api.addUser(user).pipe(...)```
- Kalder backend via service: UserApiService.addUser(user)
- Returnerer en Observable med den nyoprettede bruger (User).

```map((createdUser: User) => UserActions.addUserSuccess({ user: createdUser }))```
- Når API’et returnerer succes, laver vi en ny action: addUserSuccess.
- Tuborg-klammerne { user: createdUser } er nødvendige, fordi addUserSuccess er defineret med props<{ user: User }>().
- Denne action dispatcher NgRx automatisk videre, fordi createEffect gør det som default.

```catchError(error => of(UserActions.addUserFailure({ error: error.message })))```
- Hvis API’et fejler (fx HTTP 400/500), fanges fejlen.
- Vi returnerer en observable af en action med of(), så streamen ikke bryder.
- addUserFailure har payload { error: string }.

Forløb i pipe
```bash
 [Component] dispatch(addUser({ user: ... }))
        │
        ▼
[Effect] ofType(addUser) filtrerer actionen
        │
        ▼
mergeMap → kalder api.addUser(user)
        │
   ┌──────────────┐
   │ Success      │
   ▼              │
dispatch(addUserSuccess({ user: createdUser }))
   │
Reducer opdaterer state.users
   │
   ▼
   UI viser ny bruger
   │
   └─────────────┐
   │ Failure      │
   ▼
dispatch(addUserFailure({ error }))
Reducer opdaterer state.errorMessage

 ```
