# HybridAuth

### Особливості:

+ використовується бібліотека `HybridAuth`, яка реалізує авторизацію через безліч соцмереж (провайдерів) без посередників;
+ Реєстрація (в тому числі з додаванням в групи) і авторизація користувачів;
+ Прив'язка декількох соцмереж до одного користувача;
+ Управління прив'язаними соцмережами з адмінки.

#### Встановлення

Завантажувати тут: [github.com/Pathologic/EvoHybridAuth](https://github.com/Pathologic/EvoHybridAuth)

Після встановлення потрібно зайти на сторінку веб-користувача в адмінці, щоб створилася таблиця. Потім перейменувати файл `config.sample` в `config.php` в папці `assets/plugins/hybridauth/config` і налаштувати в ньому необхідні соцмережі. По налаштуванню доведеться гуглити, але щось можна знайти тут: `docs.modx.pro/components/hybridauth/providers/`
Також потрібно включити плагін `userHelper`.

Класи для основних провайдерів знаходяться в папці `assets/plugins/hybridauth/vendor/hybridauth/hybridauth/hybridauth/Hybrid/Providers/` і не вимагають додаткових дій. У разі додаткових провайдерів з папки `assets/plugins/hybridauth/vendor/hybridauth/hybridauth/additional-providers/` необхідно додати в конфігурацію провайдера ключ wrapper. На прикладі Vkontakte:

```
"Vkontakte" => array(
        "enabled" => true,
        "keys" => array("id" => "", "secret" => ""),
        "wrapper" => array(
            'class'=>'Hybrid_Providers_Vkontakte',
            'path' => MODX_BASE_PATH.'assets/plugins/hybridauth/vendor/hybridauth/hybridauth/additional-providers/hybridauth-vkontakte/Providers/Vkontakte.php'
        )
    ),
```

Компонент складається з плагіна і сніпета. Cніппет виводить список доступних для використання соцмереж, а плагін забезпечує взаємодію з ними.

## Параметри плагіна

### registerUsers
дозволяє реєстрацію нових користувачів. Якщо заборонено, то авторизований користувачів повинен прив'язати в своєму профілі соцмережі і входити потім через них

### debug
включає режим налагодження, повідомлення пишуться як в лог MODX, так і в файл `assets/cache/ha/error.log`

### rememberme, cookieName, cookieLifetime
параметри для запам'ятовування користувача після авторизації, описані в документації `FormLister`

### redirectUri 
адреса сторінки, на яку користувача повертають з соцмереж, якщо не заповнювати, то буде використовуватися головна сторінка сайту; ця адреса має збігатися з тим, що вказується в налаштуваннях додатків соцмереж

### loginPage 
id сторінки, на яку користувач повинен бути перенаправлений після авторизації; якщо не заповнено, то поточна сторінка

### logoutPage
id сторінки, на яку користувач повинен бути перенаправлений після виходу; якщо не заповнено, то поточна сторінка

### groups
список груп, в які потрібно додати користувача після реєстрації, у вигляді json-масиву

### userModel
клас для роботи з користувачами, за замовчуванням — `modUsers`

### tabName
заголовок вкладки на сторінці веб-користувача в адмінці



## Параметри сніппета

### langDir, lang, lexicon
Налаштування лексиконів, див. документацію `FormLister`

### tpl 
шаблон списку провайдерів, доступні плейсхолдери `[+providers+]` і `[+error+]`; якщо значення порожнє, то повернеться масив з ключами providers і error

### providerTpl
шаблон провайдера, доступні плейсхолдери `[+url+]` (посилання для виконання дії), `[+classNames+]` (імена класів), [+title+] (назва провайдера)

### activeProviderTpl 
шаблон підключеного провайдера, якщо не вказувати, то використовується значення providerTpl

### errorTpl
шаблон повідомлення про помилку, доступний плейсхолдер `[+error+]`

### registerCss
підключає файл `assets/snippets/hybridauth/css/default.css`
