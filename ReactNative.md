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

💡 Hvis du vil, kan jeg også vise dig:

* **Den bedste React Native projektstruktur (2025)**
* **Hvordan du laver login + JWT mod dit .NET API**
* **Hvordan du bygger en rigtig mobilapp (Android + iOS)**.
