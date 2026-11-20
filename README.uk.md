<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="brand/logo/kiri-horizontal-dark.svg">
    <img src="brand/logo/kiri-horizontal-light.svg" alt="Kiri" width="220">
  </picture>
</p>

<p align="center">
  <b>Маленьке й швидке середовище виконання контейнерів для macOS, Windows і Linux.</b><br>
  Запускає ваші Docker-образи й працює з CLI <code>docker</code>, Compose і buildx, якими ви вже користуєтеся.
</p>

<p align="center">
  <a href="https://runkiri.com/releases/">Завантажити</a> ·
  <a href="docs/features.uk.md">Можливості</a> ·
  <a href="docs/development.md">Збірка з вихідного коду</a> ·
  <a href="https://github.com/libracoder/kiri/releases">Випуски</a> ·
  <a href="README.md">English</a>
</p>

<p align="center">
  <a href="https://github.com/libracoder/kiri/releases/latest"><img alt="Останній випуск" src="https://img.shields.io/github/v/release/libracoder/kiri?label=release&color=2f6feb"></a>
  <a href="LICENSE"><img alt="Ліцензія: Apache-2.0" src="https://img.shields.io/badge/license-Apache--2.0-555"></a>
  <a href="#вашою-мовою"><img alt="38 мов" src="https://img.shields.io/badge/languages-38-2ea043"></a>
</p>

<p align="center">
  🇬🇧 🇮🇩 🇩🇪 🇪🇸 🇫🇷 🇮🇹 🇵🇱 🇧🇷 🇻🇳 🇹🇷 🇷🇺 🇸🇦 🇯🇵 🇰🇷 🇨🇳 🇹🇼<br>
  <sub>Kiri розмовляє 38 мовами. <a href="#вашою-мовою">Чи є серед них ваша?</a></sub>
</p>

<p align="center">
  <img src="docs/images/kiri-window.png" alt="Вікно Kiri: контейнери, згруповані за проєктами Compose, з їхнім станом, портами, локальними доменами й розміром" width="900">
</p>

---

Kiri замінює Docker Desktop і OrbStack. Він легкий і написаний на Rust.
Контейнери запускаються в одній маленькій віртуальній машині Linux (а в
Linux напряму на ядрі системи), а Kiri підтримує API Docker Engine, тож
ваші інструменти працюють без змін.

Назва походить від слова *ntakịrị* мовою ігбо, що означає «маленький».

## Чому Kiri

- **Швидкий**: через Docker API контейнер запускається й завершується приблизно за 2,5 мс. Це приблизно у 80 разів швидше, ніж Docker Desktop.
- **Легкий**: ВМ засинає після 60 с простою й прокидається приблизно за 0,15 с, а демон займає близько 15 МБ.
- **Сумісний**: `docker`, Compose, buildx, `docker login` і образи amd64 через Rosetta.
- **Локальні домени**: `http://<name>.kiri.local`, а також HTTPS від власного центру сертифікації Kiri.
- **Не лише контейнери**: кластер Kubernetes однією командою й Linux-машини.
- **Нативне вікно** для контейнерів, образів, томів і мереж, а також значок у рядку меню.
- **Вашою мовою**: 38 мов, однаково в macOS, Windows і Linux.

### Вашою мовою

Вікно Kiri та його налаштування доступні цими мовами. Kiri бере мову системи, а змінити її можна в «Налаштування» → «Вигляд». Арабською, перською й центральнокурдською все вікно читається справа наліво.

| | Мова | | Мова | | Мова | | Мова |
|---|---|---|---|---|---|---|---|
| 🇬🇧 | English | 🇳🇴 | Norsk bokmål | 🇧🇬 | Български | 🇮🇳 | తెలుగు |
| 🇮🇩 | Bahasa Indonesia | 🇵🇱 | Polski | 🇷🇺 | Русский | 🇹🇭 | ไทย |
| 🇦🇩 | Català | 🇧🇷 | Português (Brasil) | 🇺🇦 | Українська | 🇱🇦 | ລາວ |
| 🇨🇿 | Čeština | 🇷🇴 | Română | 🇰🇿 | Қазақша | 🇯🇵 | 日本語 |
| 🇩🇪 | Deutsch | 🇦🇱 | Shqip | 🇸🇦 | العربية | 🇰🇷 | 한국어 |
| 🇪🇸 | Español | 🇸🇮 | Slovenščina | 🇮🇷 | فارسی | 🇨🇳 | 简体中文 |
| 🇫🇷 | Français | 🇫🇮 | Suomi | 🇮🇶 | کوردیی ناوەندی | 🇹🇼 | 繁體中文 |
| 🇮🇹 | Italiano | 🇻🇳 | Tiếng Việt | 🇮🇳 | हिन्दी | 🇪🇹 | አማርኛ |
| 🇱🇹 | Lietuvių | 🇹🇷 | Türkçe | 🇧🇩 | বাংলা |  |  |
| 🇭🇺 | Magyar | 🇬🇷 | Ελληνικά | 🇮🇳 | தமிழ் |  |  |

Немає вашої? Переклади лежать у [`crates/kiri/src/ui/lang`](crates/kiri/src/ui/lang); див. [Участь у розробці](#участь-у-розробці).

## Установлення

| Платформа | Де взяти |
|----------|--------|
| macOS 14+ (Apple silicon або Intel) | `.dmg` з [runkiri.com/releases](https://runkiri.com/releases/) або `brew install --cask libracoder/kiri/kiri` |
| Windows 10 і 11 (x86-64 або Arm, через WSL 2) | `.msi` з [runkiri.com/releases](https://runkiri.com/releases/) |
| Linux (Ubuntu, Fedora та інші) | `.deb` або `.rpm` з [runkiri.com/releases](https://runkiri.com/releases/) |

Під час першого відкриття Kiri створює Docker-контекст `kiri`. Kiri оновлюється сам і спершу перевіряє підпис кожного нового випуску.

## Швидкий старт

```sh
docker --context kiri run --rm alpine echo hello
docker --context kiri compose up -d
kiri k8s start                      # одновузловий Kubernetes як kubectl-контекст `kiri`
kiri machine create dev --distro ubuntu && kiri machine shell dev
kiri import                         # перенести образи, томи й контейнери з OrbStack або Docker Desktop
```

Команди Docker працюють і як команди `kiri` (`kiri run`, `kiri ps`,
`kiri compose up` тощо) і завжди звертаються до Kiri.

## Цифри

`alpine` із командою `true` і видаленням, медіана 30 запусків, той самий CLI `docker` для всіх трьох:

|                | Docker Engine API | CLI `docker` |
|----------------|------------------:|-------------:|
| **Kiri**       | **2,55 мс**       | **14,2 мс**  |
| Docker Desktop | 213 мс            | 243 мс       |
| OrbStack       | 380 мс            | 442 мс       |

Методика й усі інші показники є в [`bench/results.json`](bench/results.json). Повторити вимірювання можна командою `kiri bench --network bridge --runs 30`, коли запущено всі три рушії.

## Як це працює

```
docker CLI ─┐
compose ────┼─ Docker Engine API ─► kiri daemon ── vsock ──► guest agent (PID 1)
buildx ─────┤                         owns the VM           clone3 → container
kiri UI ────┘
```

ВМ використовує власне ядро розміром 10 МБ, а PID 1 у ній займає агент на
Rust розміром ~900 КБ. Цей агент запускає контейнери одним викликом `clone3` у
простори імен і cgroup. Тут немає runc, shim і dockerd. Докладніше в
[docs/development.md](docs/development.md#architecture) (англійською).

## Документація

- [Можливості](docs/features.uk.md): усе, що підтримується зараз, файли випусків і відомі проблеми
- [Розробка](docs/development.md) (англійською): збірка, запуск із вихідного коду, архітектура й структура репозиторію
- [Примітки до випусків](docs/releases) (англійською)

## План розвитку

Далі будуть нові способи ділитися (Tailscale і Cloudflare Tunnel у 0.3.0) і
підтримка Coolify (0.4.0). Що входить у кожен випуск, див. у
[ROADMAP.md](ROADMAP.md) (англійською).

## Участь у розробці

Issues і pull requests вітаються. Див. [CONTRIBUTING.md](CONTRIBUTING.md) і
[Кодекс поведінки](CODE_OF_CONDUCT.md) (англійською). Щоб приватно
повідомити про вразливість, див. [SECURITY.md](SECURITY.md).

## Ліцензія

[Apache-2.0](LICENSE). Ліцензії сторонніх компонентів наведено в
[NOTICE](NOTICE). У гостьовій системі працює ядро Linux (GPL-2.0); його конфігурація,
патчі й пропозиція вихідного коду лежать у [`guest/kernel`](guest/kernel).

Ліцензія не поширюється на назву «Kiri» і логотип Kiri. Форки вітаються,
але під іншою назвою й з іншим логотипом.
