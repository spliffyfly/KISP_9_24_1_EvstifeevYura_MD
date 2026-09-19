# Знакомство с Expo

> Конспект по официальной документации [docs.expo.dev](https://docs.expo.dev/).
> **Expo CLI** позволяет разрабатывать, собирать (компилировать) и запускать приложение, а также выполнять множество других задач в рамках одного инструмента.

## Содержание

1. [Create a project (Создание проекта)](#1-create-a-project-создание-проекта)
2. [Set up your environment (Настройка окружения)](#2-set-up-your-environment-настройка-окружения)
3. [Start developing (Начало разработки)](#3-start-developing-начало-разработки)
4. [Next steps (Следующие шаги)](#4-next-steps-следующие-шаги)
5. [Tools for development (Инструменты разработки)](#5-tools-for-development-инструменты-разработки)
6. [Navigation (Навигация)](#6-navigation-навигация)

---

## 1. Create a project (Создание проекта)

**Expo** — это фреймворк на базе React Native, упрощающий разработку приложений под Android и iOS. Даёт файловый роутинг, стандартный набор нативных модулей "из коробки" и многое другое.

**Системные требования:**

- Node.js (LTS-версия)
- ОС: macOS, Windows (PowerShell или WSL 2), Linux

Новый проект создаётся командой:

```bash
npx create-expo-app@latest
```

Можно выбрать другой шаблон через флаг `--template`, например пустой (`blank`) шаблон без роутинга:

```bash
npx create-expo-app@latest --template blank
```

> 💡 Также можно стартовать не с дефолтного проекта, а с одного из готовых примеров Expo (`--example`), которые демонстрируют конкретную фичу (камера, виджеты и т.д.).

---

## 2. Set up your environment (Настройка окружения)

После создания проекта нужно настроить **локальное окружение разработки** для запуска на Android/iOS.

Два основных сценария:

| Способ | Когда использовать |
| --- | --- |
| **Expo Go** | Быстрый старт, обучение — "песочница" для запуска приложения без сборки |
| **Development Build** | Полноценная сборка собственного приложения со всеми инструментами разработчика Expo |

**Рекомендация:** разрабатывать на реальном устройстве — так вы увидите именно то, что увидит пользователь.

Для Expo Go достаточно установить приложение из Google Play / App Store и отсканировать QR-код из терминала.

---

## 3. Start developing (Начало разработки)

Запуск сервера разработки:

```bash
npx expo start
```

После запуска в терминале появится **QR-код**:

- отсканируйте его приложением Expo Go на телефоне;
- либо нажмите `a` — откроется Android Emulator;
- либо нажмите `i` — откроется iOS Simulator.

**Требование:** компьютер и устройство должны быть в одной Wi-Fi сети.

Если сеть не позволяет (например, публичный Wi-Fi), используйте туннель:

```bash
npx expo start --tunnel
```

> ⚠️ Режим `--tunnel` заметно медленнее, чем `LAN`/`Local`, поэтому используйте его только при необходимости.

Изменения в коде (например, в `app/index.tsx`) применяются "на лету" благодаря **Fast Refresh**.

---

## 4. Next steps (Следующие шаги)

После первого запуска проект готов к полноценной разработке. Дальнейший путь:

- **Develop** — изучить инструменты разработки и добавить функциональность (навигация, БД, авторизация).
- **Review** — протестировать приложение (unit-тесты через Jest, отладка).
- **Deploy / Submit** — собрать и опубликовать приложение через **EAS** (Expo Application Services):
  - `EAS Build` — сборка и подпись Android/iOS приложений;
  - `EAS Submit` — публикация в Google Play и App Store;
  - `EAS Update` — публикация обновлений "по воздуху" (OTA);
  - `EAS Hosting` — деплой веб-версии (Expo Router).

---

## 5. Tools for development (Инструменты разработки)

### Expo CLI

Устанавливается автоматически вместе с пакетом `expo`. Запускается через `npx`.

Основные команды:

| Команда | Описание |
| --- | --- |
| `npx expo start` | Запуск сервера разработки (Expo Go / Development Build) |
| `npx expo prebuild` | Генерация нативных папок `android` и `ios` (Continuous Native Generation) |
| `npx expo install <пакет>` | Установка пакета с учётом совместимости версии SDK |
| `npx expo lint` | Настройка ESLint для проекта |
| `npx expo export` | Экспорт проекта для production |

### Дополнительные инструменты

- **Expo Orbit / Dev Tools** — управление симуляторами и устройствами;
- **VS Code + расширения** — автодополнение и intellisense для `app.json`;
- **Expo Snack** — онлайн-песочница для быстрых экспериментов без локальной установки.

---

## 6. Navigation (Навигация)

React Native **не включает** встроенную навигацию — её нужно подключать отдельной библиотекой. Для Expo-проектов есть два основных варианта:

### React Navigation

- Компонентный подход — навигаторы (stack, tabs, drawer) описываются в коде;
- Гибкая кастомизация переходов и анимаций;
- Подходит для сложных, нестандартных сценариев UX.

### Expo Router *(рекомендуемый способ)*

- **Файловый роутинг**: каждый файл в директории `app/` становится экраном/маршрутом;
- Построен поверх React Navigation, но не требует ручного описания навигаторов;
- Работает одинаково на Android, iOS и web;
- Поддерживает вложенные layout-файлы `_layout.tsx`, типизированные маршруты, deep linking.

Пример структуры проекта с табами:

```text
app/
├── _layout.tsx
└── (tabs)/
    ├── _layout.tsx
    ├── index.tsx
    └── settings.tsx
```

Пример навигации между экранами через хук `useRouter`:

```tsx
import { useRouter } from 'expo-router';
import { Button } from 'react-native';

export default function Home() {
  const router = useRouter();
  return <Button title="Перейти в профиль" onPress={() => router.navigate('/profile')} />;
}
```

> Начиная с шаблона по умолчанию (`create-expo-app@latest`), **Expo Router уже встроен** в новый проект.

---

## Полезные ссылки

- [Официальная документация Expo](https://docs.expo.dev/)
- [Create a project](https://docs.expo.dev/get-started/create-a-project/)
- [Set up your environment](https://docs.expo.dev/get-started/set-up-your-environment/)
- [Start developing](https://docs.expo.dev/get-started/start-developing/)
- [Next steps](https://docs.expo.dev/get-started/next-steps/)
- [Tools for development](https://docs.expo.dev/develop/tools/)
- [Navigation](https://docs.expo.dev/develop/app-navigation/)

---

# Туториал: StickerSmash (React Native и Expo)

> Источник: [docs.expo.dev/tutorial/introduction](https://docs.expo.dev/tutorial/introduction/)
>
> Это практический туториал от Expo, в котором с нуля создаётся универсальное приложение **StickerSmash** (Android, iOS, web) — выбор фото, добавление стикеров-эмодзи, жесты, сохранение скриншота.

## Содержание туториала

1. [Introduction (Введение)](#1-introduction-введение)
2. [Create your first app (Создание первого приложения)](#2-create-your-first-app-создание-первого-приложения)
3. [Add navigation (Добавление навигации)](#3-add-navigation-добавление-навигации)
4. [Build a screen (Создание экрана)](#4-build-a-screen-создание-экрана)
5. [Use an image picker (Использование выбора изображений)](#5-use-an-image-picker-использование-выбора-изображений)
6. [Create a modal (Создание модального окна)](#6-create-a-modal-создание-модального-окна)
7. [Add gestures (Добавление жестов)](#7-add-gestures-добавление-жестов)
8. [Take a screenshot (Создание скриншота)](#8-take-a-screenshot-создание-скриншота)
9. [Handle platform differences (Обработка различий платформ)](#9-handle-platform-differences-обработка-различий-платформ)
10. [Configure status bar, splash screen and app icon (Настройка статус-бара, заставки и иконки)](#10-configure-status-bar-splash-screen-and-app-icon-настройка-статус-бара-заставки-и-иконки)
11. [Learning resources (Дополнительные материалы)](#11-learning-resources-дополнительные-материалы)

---

## 1. Introduction (Введение)

Цель туториала — познакомиться с Expo SDK на практике. За ~2 часа он проведёт через создание приложения **StickerSmash**, которое:

- работает на Android, iOS и web из одной кодовой базы;
- использует `TypeScript` и стандартный шаблон `create-expo-app`;
- реализует двухэкранный **bottom tabs**-layout через **Expo Router**;
- строит вёрстку через **Flexbox**;
- выбирает изображение из галереи устройства;
- показывает модальное окно `<Modal>` со списком эмодзи (`<FlatList>`);
- добавляет к стикеру жесты (перетаскивание, двойной тап);
- делает скриншот и сохраняет его на устройство;
- корректно обрабатывает различия между платформами (Android/iOS/web);
- настраивает статус-бар, splash screen и иконку приложения.

Полный исходный код доступен на GitHub.

---

## 2. Create your first app (Создание первого приложения)

**Предварительные требования:**

- Установленный **Expo Go** на телефоне;
- **Node.js (LTS)**;
- Редактор кода (например, VS Code);
- Терминал (macOS, Linux, Windows PowerShell/WSL2).

**Инициализация проекта:**

```bash
# Создаём проект с именем StickerSmash
npx create-expo-app@latest StickerSmash

# Переходим в папку проекта
cd StickerSmash
```

Команда создаёт проект на основе **дефолтного шаблона**, который включает:

- React Native проект с установленным пакетом `expo`;
- рекомендованные инструменты (Expo CLI);
- базовую tab-навигацию от Expo Router;
- автоматическую настройку под Android, iOS и web;
- готовую конфигурацию TypeScript.

**Загрузка ассетов:** для туториала нужно скачать архив с изображениями и распаковать его в `assets/images`, заменив файлы с такими же именами.

---

## 3. Add navigation (Добавление навигации)

**Основы Expo Router:**

- `app/` — специальная директория: каждый файл внутри становится экраном (в native) и страницей (на web);
- `app/_layout.tsx` — корневой layout, задаёт общий UI (шапки, таб-бар) для всех маршрутов;
- **Stack-навигатор** — основа перемещения между экранами. На Android новый экран анимированно накладывается сверху, на iOS — выезжает справа.

**Переход между экранами** реализуется через компонент `Link`:

```tsx
import { Link } from 'expo-router';

<Link href="/about" style={{ color: 'blue' }}>
  Go to About screen
</Link>
```

**Структура каталогов с табами:**

```text
app
├── _layout.tsx        # Корневой layout
├── +not-found.tsx      # обрабатывает несуществующие маршруты (404)
└── (tabs)
    ├── _layout.tsx      # Layout таб-бара
    ├── index.tsx        # маршрут '/'
    └── about.tsx        # маршрут '/about'
```

Корневой layout оборачивает `(tabs)` в `Stack`:

```tsx
import { Stack } from 'expo-router';

export default function RootLayout() {
  return (
    <Stack>
      <Stack.Screen name="(tabs)" options={{ headerShown: false }} />
    </Stack>
  );
}
```

---

## 4. Build a screen (Создание экрана)

Собирается первый экран приложения: большое изображение по центру и две кнопки внизу.

**Разбивка UI на элементы:**

- большое изображение по центру экрана;
- две кнопки в нижней половине экрана:
  - первая — с жёлтой рамкой, иконкой и текстом ("выбрать фото");
  - вторая — простая кнопка ("использовать это фото").

Для отображения изображения используется компонент `Image` из библиотеки **`expo-image`**, а для кнопок — `Pressable` из React Native. Верстка строится с помощью **Flexbox** (`flex`, `alignItems`, `justifyContent`).

> 💡 Кнопку выносят в переиспользуемый компонент `Button`, чтобы не дублировать код.

---

## 5. Use an image picker (Использование выбора изображений)

React Native не имеет встроенного компонента для выбора изображений из галереи — для этого используется библиотека Expo SDK **`expo-image-picker`**.

**Установка:**

```bash
npx expo install expo-image-picker
```

**Использование `launchImageLibraryAsync()`:**

```tsx
import * as ImagePicker from 'expo-image-picker';

export default function Index() {
  const pickImageAsync = async () => {
    let result = await ImagePicker.launchImageLibraryAsync({
      mediaTypes: ['images'],
      allowsEditing: true,
      quality: 1,
    });

    if (!result.canceled) {
      console.log(result);
    } else {
      alert('You did not select any image.');
    }
  };

  // ...остальной код
}
```

- `mediaTypes` — какие типы медиа разрешено выбирать (изображения, видео);
- `allowsEditing` — разрешить обрезку изображения перед подтверждением;
- `result.canceled` — флаг, что пользователь закрыл диалог без выбора файла.

---

## 6. Create a modal (Создание модального окна)

React Native предоставляет компонент **`<Modal>`**, отображающий контент поверх остального интерфейса — обычно для привлечения внимания к важному действию.

В этом разделе создаётся модальное окно со списком эмодзи-стикеров (`<FlatList>`), которое пользователь открывает нажатием круглой кнопки.

**Ключевые шаги:**

1. Добавить состояние `showAppOptions` (показывать ли новые кнопки после выбора фото) и `isModalVisible` (видимость модалки).
2. Показать компонент `<Modal>` с обработчиком закрытия `onClose`.
3. Внутри модалки вывести `EmojiList` — список эмодзи через `<FlatList>` с горизонтальной прокруткой.
4. При выборе эмодзи — закрыть модалку и разместить стикер на изображении.

```tsx
const [isModalVisible, setIsModalVisible] = useState<boolean>(false);

const onAddSticker = () => {
  setIsModalVisible(true);
};

const onModalClose = () => {
  setIsModalVisible(false);
};
```

---

## 7. Add gestures (Добавление жестов)

Для жестов используются две библиотеки:

- **[React Native Gesture Handler](https://docs.swmansion.com/react-native-gesture-handler/docs/)** — распознаёт нативные жесты (pan, tap, rotation и т.д.);
- **[Reanimated](https://docs.swmansion.com/react-native-reanimated/docs/fundamentals/handling-gestures/)** — анимирует переходы между состояниями жестов.

**Реализуются два жеста:**

- **двойной тап** — увеличивает/уменьшает размер стикера;
- **перетаскивание (pan)** — позволяет перемещать стикер по экрану.

**Установка:**

```bash
npx expo install react-native-gesture-handler react-native-reanimated
```

**Оборачивание корневого компонента:**

```tsx
import { GestureHandlerRootView } from 'react-native-gesture-handler';

export default function Index() {
  return (
    <GestureHandlerRootView style={styles.container}>
      {/* остальной код */}
    </GestureHandlerRootView>
  );
}
```

Для анимации стикера компонент оборачивается в `Animated.Image` из `react-native-reanimated`, а жесты описываются через `Gesture.Tap()` и `Gesture.Pan()`.

---

## 8. Take a screenshot (Создание скриншота)

Для сохранения готового изображения со стикером используются две библиотеки:

- **`react-native-view-shot`** — делает снимок (`captureRef()`) любого `<View>` в приложении;
- **`expo-media-library`** — сохраняет полученный файл в медиатеку устройства.

**Установка:**

```bash
npx expo install react-native-view-shot expo-media-library
```

**Основные шаги:**

1. Запросить разрешение на доступ к медиатеке через `MediaLibrary.usePermissions()`.
2. Создать `imageRef` (`useRef<View>`) и обернуть им блок с изображением и стикером; указать `collapsable={false}`, чтобы `View` не "схлопывался" и корректно попадал в скриншот.
3. В функции сохранения вызвать `captureRef(imageRef, { height, quality })`, получить `uri`.
4. Сохранить файл через `MediaLibrary.saveToLibraryAsync(uri)`.

> 💡 Библиотек для решения нестандартных задач в React Native очень много — их можно найти на [React Native Directory](https://reactnative.directory/).

---

## 9. Handle platform differences (Обработка различий платформ)

Не все возможности одинаково доступны на Android, iOS и web. Например, `react-native-view-shot` работает в нативных приложениях, но **не работает в браузере**.

**Решение:** для web используется отдельная библиотека **`dom-to-image`**, которая делает скриншот DOM-узла и конвертирует его в SVG/PNG/JPEG.

**Установка (только через npm, не `expo install`):**

```bash
npm install dom-to-image
```

**Определение платформы через модуль `Platform`:**

```tsx
import { Platform } from 'react-native';
import domtoimage from 'dom-to-image';

const onSaveImageAsync = async () => {
  if (Platform.OS !== 'web') {
    // логика для Android / iOS (react-native-view-shot)
  } else {
    try {
      const dataUrl = await domtoimage.toJpeg(imageRef.current, {
        quality: 0.95,
        width: 320,
        height: 440,
      });

      let link = document.createElement('a');
      link.download = 'sticker-smash.jpeg';
      link.href = dataUrl;
      link.click();
    } catch (e) {
      console.log(e);
    }
  }
};
```

- `Platform.OS` — возвращает строку `'ios'`, `'android'` или `'web'`;
- логика ветвится, но UI и остальной код остаются общими для всех платформ.

---

## 10. Configure status bar, splash screen and app icon (Настройка статус-бара, заставки и иконки)

Финальный этап — довести приложение "до товарного вида" перед публикацией в сторы.

### Статус-бар

Библиотека **`expo-status-bar`** предустановлена в каждом проекте `create-expo-app`.

```tsx
import { Stack } from 'expo-router';
import { StatusBar } from 'expo-status-bar';

export default function RootLayout() {
  return (
    <>
      <Stack>
        <Stack.Screen name="(tabs)" options={{ headerShown: false }} />
      </Stack>
      <StatusBar style="light" />
    </>
  );
}
```

### Иконка приложения

- Путь к иконке (`1024×1024 px`, `.png`) задаётся в `app.json` через свойство `"icon"`;
- по умолчанию уже указывает на `./assets/images/icon.png`;
- при сборке через **EAS** Expo автоматически генерирует оптимизированные иконки под каждое устройство.

### Splash screen (заставка)

- Настраивается через конфиг-плагин `expo-splash-screen` в `app.json`:

```json
{
  "plugins": [
    [
      "expo-splash-screen",
      {
        "image": "./assets/images/splash-icon.png"
      }
    ]
  ]
}
```

> ⚠️ **Важно:** заставку **нельзя протестировать** через Expo Go или Development Build — нужна **preview** или **production**-сборка через EAS.

---

## 11. Learning resources (Дополнительные материалы)

После завершения туториала рекомендуется углубиться в технологии, использованные в проекте:

- **React** — [Quick Start](https://react.dev/learn) и [Hooks](https://react.dev/reference/react/hooks) в официальной документации React.
- **React Native** — [React Native basics](https://reactnative.dev/docs/getting-started), а также API-справочники: `View`, `Text`, [platform-specific code](https://reactnative.dev/docs/platform-specific-code), [списки данных](https://reactnative.dev/docs/using-a-listview).
- **Flexbox** — для более глубокого понимания вёрстки.
- **Жесты и анимации** — документация React Native Gesture Handler и Reanimated.
- **[Отладка (Debugging)](https://docs.expo.dev/debugging/runtime-issues/)** — инструменты для поиска и исправления ошибок.
- **Сообщество** — [Discord Expo](https://chat.expo.dev) для общения с другими разработчиками и вопросов.

---

## Полезные ссылки по туториалу

- [Introduction](https://docs.expo.dev/tutorial/introduction/)
- [Create your first app](https://docs.expo.dev/tutorial/create-your-first-app/)
- [Add navigation](https://docs.expo.dev/tutorial/add-navigation/)
- [Build a screen](https://docs.expo.dev/tutorial/build-a-screen/)
- [Use an image picker](https://docs.expo.dev/tutorial/image-picker/)
- [Create a modal](https://docs.expo.dev/tutorial/create-a-modal/)
- [Add gestures](https://docs.expo.dev/tutorial/gestures/)
- [Take a screenshot](https://docs.expo.dev/tutorial/screenshot/)
- [Handle platform differences](https://docs.expo.dev/tutorial/platform-differences/)
- [Configure status bar, splash screen and app icon](https://docs.expo.dev/tutorial/configuration/)
- [Learning resources](https://docs.expo.dev/tutorial/follow-up/)
