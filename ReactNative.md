# Kom igang med React Native

---

# 1️⃣ Installer det nødvendige

Du skal først installere **Node.js**.

1. Download Node.js (LTS-version)
2. Installer globalt Expo CLI:

```bash
npm install -g expo-cli
```

Tjek installation:

```bash
node -v
npm -v
expo --version
```

---

# 2️⃣ Opret dit første React Native projekt

Kør:

```bash
npx create-expo-app MyFirstApp
```

Gå ind i mappen:

```bash
cd MyFirstApp
```

Start appen:

```bash
npm start
```

Det åbner **Expo Go** via en QR-kode.

---

# 3️⃣ Test appen på din telefon

1. Installer **Expo Go** på din telefon
2. Scan QR-koden i terminalen
3. Appen kører med det samme 📱

Du behøver ikke Android Studio eller Xcode i starten.

---

# 4️⃣ Din første React Native kode

`App.js`

```javascript
import { Text, View } from 'react-native';

export default function App() {
  return (
    <View>
      <Text>Hello World</Text>
    </View>
  );
}
```

Grundlæggende komponenter:

* `View` → som `div`
* `Text` → tekst
* `Image`
* `ScrollView`
* `TouchableOpacity` → klik

---

# 5️⃣ Styling

React Native bruger **JavaScript styles i stedet for CSS**.

```javascript
import { StyleSheet, Text, View } from 'react-native';

export default function App() {
  return (
    <View style={styles.container}>
      <Text>Hello React Native</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center'
  }
});
```

---

# 6️⃣ Navigation (meget vigtigt i apps)

Installer **React Navigation**:

```bash
npm install @react-navigation/native
```

Det bruges til:

* Stack navigation
* Tab navigation
* Drawer navigation

---

# 7️⃣ Kald dit API

Da du allerede arbejder med API’er i .NET, kan du nemt bruge dem:

```javascript
fetch("https://api.myside.com/users")
  .then(res => res.json())
  .then(data => console.log(data));
```

Eller med **Axios**.

---

# 8️⃣ Struktur på et projekt

Typisk struktur:

```
/components
/screens
/navigation
/services
/App.js
```

Eksempel:

```
screens/
  HomeScreen.js
  LoginScreen.js

services/
  api.js
```

---

# 9️⃣ Når du bliver mere avanceret

Senere kan du tilføje:

* **Redux** eller **Zustand**
* **Firebase**
* Native modules
* Push notifications

---

💡 **Tip til dig:**
Da du allerede bruger **Angular**, vil React Native føles meget som:

| Angular    | React Native     |
| ---------- | ---------------- |
| Components | Components       |
| Services   | Hooks / Services |
| HTTPClient | fetch / axios    |

---

✅ **Den bedste måde at lære det hurtigt på:**
Lav en lille app med:

* Login screen
* Register screen
* API til dit backend
* JWT login

(det matcher dine eksisterende projekter).

---

Her er en **god og realistisk React Native projektstruktur**, som bruges i mange produktion-apps. Den passer også godt til din måde at arbejde på med **API’er og login-systemer**. 🚀

Frameworket er stadig **React Native**, og navigation håndteres typisk med **React Navigation**.

---

# 📁 God React Native projektstruktur

```
src/
 ├── components/
 │    ├── Button.js
 │    ├── Input.js
 │
 ├── screens/
 │    ├── LoginScreen.js
 │    ├── RegisterScreen.js
 │    ├── HomeScreen.js
 │
 ├── navigation/
 │    ├── AppNavigator.js
 │
 ├── services/
 │    ├── api.js
 │    ├── authService.js
 │
 ├── context/
 │    ├── AuthContext.js
 │
 ├── hooks/
 │
 ├── utils/
 │
 └── App.js
```

Forklaring:

| Mappe      | Formål                      |
| ---------- | --------------------------- |
| components | Genbrugelige UI komponenter |
| screens    | Selve app-sider             |
| navigation | Navigation setup            |
| services   | API kald                    |
| context    | Global state (login osv)    |
| hooks      | Custom React hooks          |
| utils      | Helper funktioner           |

---

# 1️⃣ Navigation

Installer:

```bash
npm install @react-navigation/native
npm install @react-navigation/native-stack
```

Eksempel:

`navigation/AppNavigator.js`

```javascript
import { NavigationContainer } from '@react-navigation/native'
import { createNativeStackNavigator } from '@react-navigation/native-stack'

import LoginScreen from '../screens/LoginScreen'
import HomeScreen from '../screens/HomeScreen'

const Stack = createNativeStackNavigator()

export default function AppNavigator() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Login" component={LoginScreen} />
        <Stack.Screen name="Home" component={HomeScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  )
}
```

---

# 2️⃣ API service

Lav et centralt API-modul.

`services/api.js`

```javascript
import axios from 'axios'

export const api = axios.create({
  baseURL: "https://api.myside.com"
})
```

Her bruges **Axios**.

Installer:

```bash
npm install axios
```

---

# 3️⃣ Login mod dit API

`services/authService.js`

```javascript
import { api } from './api'

export async function login(username, password) {
  const res = await api.post("/auth/login", {
    username,
    password
  })

  return res.data
}
```

---

# 4️⃣ Login screen

`screens/LoginScreen.js`

```javascript
import { useState } from 'react'
import { View, TextInput, Button } from 'react-native'
import { login } from '../services/authService'

export default function LoginScreen({ navigation }) {

  const [username, setUsername] = useState("")
  const [password, setPassword] = useState("")

  const handleLogin = async () => {

    const result = await login(username, password)

    if(result.token){
      navigation.navigate("Home")
    }
  }

  return (
    <View>

      <TextInput
        placeholder="Username"
        value={username}
        onChangeText={setUsername}
      />

      <TextInput
        placeholder="Password"
        secureTextEntry
        value={password}
        onChangeText={setPassword}
      />

      <Button title="Login" onPress={handleLogin} />

    </View>
  )
}
```

---

# 5️⃣ Gem JWT token

I mobilapps gemmes tokens ofte med **AsyncStorage**.

Installer:

```bash
npm install @react-native-async-storage/async-storage
```

Eksempel:

```javascript
import AsyncStorage from '@react-native-async-storage/async-storage'

await AsyncStorage.setItem("token", token)
```

Hent token:

```javascript
const token = await AsyncStorage.getItem("token")
```

---

# 6️⃣ Send token automatisk til API

Tilføj en Axios interceptor.

`api.js`

```javascript
import axios from 'axios'
import AsyncStorage from '@react-native-async-storage/async-storage'

export const api = axios.create({
  baseURL: "https://api.myside.com"
})

api.interceptors.request.use(async config => {

  const token = await AsyncStorage.getItem("token")

  if(token){
    config.headers.Authorization = `Bearer ${token}`
  }

  return config
})
```

---

# 7️⃣ Typisk arkitektur for større apps

```
src
 ├── components
 ├── screens
 ├── navigation
 ├── services
 ├── context
 ├── hooks
 ├── store
 ├── assets
 └── utils
```

Store bruges ofte til state management med:

* **Redux**
* **Zustand**

---

# 8️⃣ Ting du næsten altid installerer

```bash
npm install axios
npm install @react-navigation/native
npm install @react-navigation/native-stack
npm install @react-native-async-storage/async-storage
```

---

# 💡 Arkitektur-tip til dig (baseret på din erfaring)

Da du arbejder med:

* .NET API
* login system
* JWT

kan du bygge en mobilapp sådan:

```
React Native App
       │
       │ JWT
       ▼
.NET API
       │
       ▼
SQL Server
```

Det er **meget almindelig enterprise arkitektur**.

---

Super 👍 Her er en **rigtig produktion-klar arkitektur til React Native + .NET API + JWT + Refresh Tokens**. Den minder faktisk meget om det login-system du tidligere har arbejdet med. 🚀

Frontend er **React Native** og navigation styres ofte med **React Navigation**.

---

# 🧱 Overordnet arkitektur

```text
React Native App
      │
      │  JWT Access Token
      │
      ▼
.NET API
      │
      ▼
SQL Server
```

Login flow:

```
Login
 ↓
API returnerer:
- Access Token (kort levetid)
- Refresh Token (lang levetid)

 ↓
App gemmer tokens
 ↓
API kald bruger Access Token
 ↓
Hvis token udløber
 ↓
App bruger Refresh Token
 ↓
nyt Access Token
```

---

# 📁 Produktion projektstruktur

```text
src
 ├── api
 │    ├── apiClient.js
 │    ├── authApi.js
 │
 ├── components
 │
 ├── navigation
 │    └── AppNavigator.js
 │
 ├── screens
 │    ├── LoginScreen.js
 │    ├── RegisterScreen.js
 │    ├── HomeScreen.js
 │
 ├── services
 │    └── tokenService.js
 │
 ├── context
 │    └── AuthContext.js
 │
 ├── hooks
 │
 ├── utils
 │
 └── App.js
```

---

# 1️⃣ API client (central HTTP client)

`api/apiClient.js`

```javascript
import axios from "axios"
import { getAccessToken } from "../services/tokenService"

export const apiClient = axios.create({
  baseURL: "https://yourapi.com"
})

apiClient.interceptors.request.use(async config => {

  const token = await getAccessToken()

  if(token){
    config.headers.Authorization = `Bearer ${token}`
  }

  return config
})
```

Her bruger vi **Axios**.

---

# 2️⃣ Token service

Mobilapps gemmer tokens lokalt.

Det gøres ofte med
**AsyncStorage**.

`services/tokenService.js`

```javascript
import AsyncStorage from "@react-native-async-storage/async-storage"

export async function saveTokens(accessToken, refreshToken){
  await AsyncStorage.setItem("accessToken", accessToken)
  await AsyncStorage.setItem("refreshToken", refreshToken)
}

export async function getAccessToken(){
  return await AsyncStorage.getItem("accessToken")
}

export async function getRefreshToken(){
  return await AsyncStorage.getItem("refreshToken")
}

export async function clearTokens(){
  await AsyncStorage.removeItem("accessToken")
  await AsyncStorage.removeItem("refreshToken")
}
```

---

# 3️⃣ Login API

`api/authApi.js`

```javascript
import { apiClient } from "./apiClient"
import { saveTokens } from "../services/tokenService"

export async function login(username, password){

  const res = await apiClient.post("/auth/login", {
    username,
    password
  })

  const { accessToken, refreshToken } = res.data

  await saveTokens(accessToken, refreshToken)

  return res.data
}
```

---

# 4️⃣ Refresh token system

Hvis token udløber, skal appen automatisk hente et nyt.

Tilføj interceptor:

`apiClient.js`

```javascript
import { getRefreshToken, saveTokens } from "../services/tokenService"

apiClient.interceptors.response.use(
  response => response,
  async error => {

    if(error.response.status === 401){

      const refreshToken = await getRefreshToken()

      const res = await axios.post(
        "https://yourapi.com/auth/refresh",
        { refreshToken }
      )

      await saveTokens(res.data.accessToken, res.data.refreshToken)

      error.config.headers.Authorization =
        "Bearer " + res.data.accessToken

      return axios(error.config)
    }

    return Promise.reject(error)
  }
)
```

Nu refreshes tokens **automatisk**.

---

# 5️⃣ Auth Context (global login state)

`context/AuthContext.js`

```javascript
import { createContext, useState } from "react"

export const AuthContext = createContext()

export function AuthProvider({ children }){

  const [user, setUser] = useState(null)

  return (
    <AuthContext.Provider value={{ user, setUser }}>
      {children}
    </AuthContext.Provider>
  )
}
```

---

# 6️⃣ Navigation med login

`navigation/AppNavigator.js`

```javascript
import { NavigationContainer } from "@react-navigation/native"
import { createNativeStackNavigator } from "@react-navigation/native-stack"

import LoginScreen from "../screens/LoginScreen"
import HomeScreen from "../screens/HomeScreen"

const Stack = createNativeStackNavigator()

export default function AppNavigator(){

  return (
    <NavigationContainer>
      <Stack.Navigator>

        <Stack.Screen name="Login" component={LoginScreen}/>
        <Stack.Screen name="Home" component={HomeScreen}/>

      </Stack.Navigator>
    </NavigationContainer>
  )
}
```

---

# 7️⃣ Login screen

`screens/LoginScreen.js`

```javascript
import { useState } from "react"
import { View, TextInput, Button } from "react-native"
import { login } from "../api/authApi"

export default function LoginScreen({ navigation }){

  const [username, setUsername] = useState("")
  const [password, setPassword] = useState("")

  const handleLogin = async () => {

    await login(username, password)

    navigation.navigate("Home")
  }

  return (
    <View>

      <TextInput
        placeholder="Username"
        value={username}
        onChangeText={setUsername}
      />

      <TextInput
        placeholder="Password"
        secureTextEntry
        value={password}
        onChangeText={setPassword}
      />

      <Button title="Login" onPress={handleLogin} />

    </View>
  )
}
```

---

# 🔐 Ekstra sikkerhed (best practice)

I produktion bruger man ofte:

* **Expo SecureStore** i stedet for AsyncStorage
* Device binding
* Token rotation
* Biometric login
* Refresh token blacklist

---

Her er en **realistisk trin-for-trin plan til at bygge en simpel men “rigtig” mobilapp på ca. 2-3 timer** med **React Native** og **Expo**. 📱🚀

Appen vi bygger:
**Login → Hent data fra API → Vis liste**

Det matcher også godt din erfaring med **API og backend**.

---

# ⏱️ 0-15 min — Opret projekt

Installer **Node.js** hvis du ikke har det.

Opret projekt:

```bash
npx create-expo-app MyApp
```

Gå ind i projektet:

```bash
cd MyApp
```

Start appen:

```bash
npm start
```

Scan QR med **Expo Go**.

Nu kører din første app på telefonen 📱

---

# ⏱️ 15-30 min — Installer nødvendige libraries

Installer navigation og HTTP client:

```bash
npm install @react-navigation/native
npm install @react-navigation/native-stack
npm install axios
```

Libraries:

* **React Navigation** → skærme/navigation
* **Axios** → API kald

---

# ⏱️ 30-60 min — Lav skærme

Lav mapper:

```
/src
  /screens
  /api
  /navigation
```

Screens:

```
LoginScreen.js
HomeScreen.js
```

---

## Login screen

```javascript
import { useState } from "react"
import { View, TextInput, Button } from "react-native"

export default function LoginScreen({ navigation }) {

  const [username, setUsername] = useState("")
  const [password, setPassword] = useState("")

  const login = () => {
    navigation.navigate("Home")
  }

  return (
    <View>

      <TextInput
        placeholder="Username"
        value={username}
        onChangeText={setUsername}
      />

      <TextInput
        placeholder="Password"
        secureTextEntry
        value={password}
        onChangeText={setPassword}
      />

      <Button title="Login" onPress={login} />

    </View>
  )
}
```

---

# ⏱️ 60-90 min — Navigation

`navigation/AppNavigator.js`

```javascript
import { NavigationContainer } from "@react-navigation/native"
import { createNativeStackNavigator } from "@react-navigation/native-stack"

import LoginScreen from "../screens/LoginScreen"
import HomeScreen from "../screens/HomeScreen"

const Stack = createNativeStackNavigator()

export default function AppNavigator(){

  return (
    <NavigationContainer>
      <Stack.Navigator>

        <Stack.Screen name="Login" component={LoginScreen} />
        <Stack.Screen name="Home" component={HomeScreen} />

      </Stack.Navigator>
    </NavigationContainer>
  )
}
```

`App.js`

```javascript
import AppNavigator from "./src/navigation/AppNavigator"

export default function App() {
  return <AppNavigator />
}
```

Nu har du en **rigtig multi-screen mobilapp**.

---

# ⏱️ 90-120 min — API kald

`api/api.js`

```javascript
import axios from "axios"

export const api = axios.create({
  baseURL: "https://jsonplaceholder.typicode.com"
})
```

---

## Hent data

`HomeScreen.js`

```javascript
import { useEffect, useState } from "react"
import { View, Text } from "react-native"
import { api } from "../api/api"

export default function HomeScreen(){

  const [posts, setPosts] = useState([])

  useEffect(() => {

    api.get("/posts")
      .then(res => setPosts(res.data))

  }, [])

  return (
    <View>

      {posts.slice(0,5).map(p => (
        <Text key={p.id}>{p.title}</Text>
      ))}

    </View>
  )
}
```

Nu har din app:

✔ Login screen
✔ Navigation
✔ API kald
✔ Data visning

Det er faktisk **en rigtig mobilapp**.

---

# ⏱️ 120-180 min — Gør appen bedre

Tilføj:

### 1️⃣ Loading state

```javascript
const [loading,setLoading] = useState(true)
```

### 2️⃣ List view

Brug:

```javascript
FlatList
```

### 3️⃣ Styling

```javascript
StyleSheet
```

---

# 🎯 Resultat efter 2-3 timer

Du har nu:

📱 Mobilapp
📡 API integration
🧭 Navigation
📄 Flere screens

Det er præcis den arkitektur som bruges i mange apps.

---

# 💡 Næste trin (meget vigtigt)

Når du kan det her, bør du lære:

1️⃣ JWT login
2️⃣ Refresh tokens
3️⃣ Global state
4️⃣ Production build

---




