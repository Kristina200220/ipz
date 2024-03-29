# Робота з локальною пам'яттю та куками #
Зберігання даних у різних варіантах зберігання є дуже корисним. Але дуже важко зрозуміти, який варіант зберігання є найкращим для конкретного випадку використання. В своїй доповіді я розкажу про варіанти зберігання даних і їх використання.
# Для чого використовуються? #
Використовуються для зберігання інформації в браузері користувача, до якої можна отримати доступ навіть після переходу на нові сторінки вашого сайту.
Ці дані також зберігаються в точному браузері користувача, яким він користується, тому, якщо ваш сайт відкрито в будь-якому браузері, вони збережуть дані лише в цьому браузері на пристрої, на якому вони наразі працюють.
Це означає, що якщо ви відкриєте інший сайт пізніше в іншому браузері, даних більше не буде.
# Cookies vs. LocalStorage: у чому різниця #
Сховище LocalStorage схоже на cookies. Воно також застосовується для зберігання даних на комп'ютері користувача (у браузері). Але крім загальних подібностей є також і багато відмінностей.

- **за місцем зберігання** (куки і дані LocalStorage зберігаються на комп'ютері користувача в браузері);

- **за розміром** (cookies обмежені 4 Кбайт, а розмір LocalStorage - 5 Мбайт);

- **по включенню цих даних в HTTP-заголовок** (куки на відміну від даних локального сховища включаються до складу запиту при відправленні його на сервер, а також сервер їх може додавати у відповідь при відправці його клієнту; таким чином cookies є частиною HTTP-протоколу, і збільшують обсяг переданих даних від клієнта серверу і назад);

- **по доступності даних** (куки можна встановити як на сервері, так і на клієнті; на клієнті доступні всі куки, крім тих, у яких встановлено прапор HttpOnly; LocalStorage доступні тільки в браузері за допомогою JavaScript API);

- **за часом зберігання даних** (куки зберігаються обмежений час: до кінця сеансу або закінчення зазначеної дати, знаходження даних в локальному сховищі не обмежена за часом);

- **за зручністю використання в JavaScript** (робота з LocalStorage в JavaScript організовано набагато зручніше ніж з cookies);

- **по необхідності інформування користувачів Євросоюзу** (при використанні cookies сайт в ЄС повинен отримувати на це дозвіл від користувачів; для даних локального сховища це не потрібно);

- **за призначенням** (куки в основному використовуються для управління сеансом, персоналізації і відстеження дій користувача, в той час як LocalStorage застосовується в якості звичайного локального сховища інформації на комп'ютері користувача).

***Що використовувати: LocalStorage або cookies?***

Насправді, відповідь на це питання дуже проста. Якщо вам не потрібно відправляти дані з кожним HTTP-запитом на сервер, то в цьому випадку краще використовувати для зберігання даних LocalStorage.
 # Що таке cookies? #
 Cookies - це технологія, що дозволяє сайтам зберігати в браузері невелику порцію даних (до 4Кбайт).
 
Зазвичай ці дані використовуються на сайті для того, щоб:

- ідентифікувати користувача;
- зберегти інформацію веб-перегляду або товарів;
- запам'ятати налаштування інтерфейсу, розташування блоків, товару, доданого в кошик і багато іншого.
# Як працюють cookies #
Простими словами вони передають дані від клієнта до сервера.

1. Клієнт (веб-браузер) посилає серверу запит. Якщо в браузері є cookies, пов'язані з цим сайтом, то він їх посилає серверу в складі цього запиту.
2. Сервер отримує запит від клієнта. Якщо в складі запиту є куки, то їх можна використовувати для виконання певної логіки на сервері, підготовки користувачеві персоналізованої сторінки або для чогось іншого. Після цього відправляємо клієнту відповідь. У заголовку відповіді відправляємо веб-браузеру (клієнту) cookies, які йому потрібно буде зберегти.
3. Веб-браузер (клієнт) отримує відповідь від сервера (сторінку) і виводить його користувачеві. Куки, які прийшли з сервера, браузер зберігає в своє сховище.
# Бібліотека js-cookie #
js-cookie - бібліотека JavaScript, що надає методи для зручної роботи з cookies.

## Підключення js-cookie до сторінки ##
    <script src="/path/to/js.cookie.js"></script>
## Установка (set) cookie ##
Запис cookie здійснюється за допомогою функції set:

    // name - ключ (ім'я) куки
    // value - значення, пов'язане з ключем name
    // attributes (необов'язковий параметр) - атрибути куки в форматі об'єкта
    Cookies.set ( 'name', 'value' [, attributes]);
## Встановити cookie для всіх сторінок сайту ##
    Cookies.set ( 'nameCookie', 'valueCookie'); // => "nameCookie = valueCookie; path = /"
## Створити cookie, дійсну 30 днів (від поточного моменту часу) і видиму будь-якими сторінками сайту ##
    Cookies.set ( 'nameCookie', 'valueCookie', {expires: 30}); // => "nameCookie = valueCookie; path = /; expires = Thu, 18 May 2021 13:00:15 GMT"
## Виконати запис куки, доступ до якої матиме тільки поточна сторінка (термін зберігання 365 днів) ##
    Cookies.set ( 'nameCookie', 'valueCookie', {expires: 365, path: ''}); // => "nameCookie = valueCookie; expires = Wed, 18 May 2021 13:00:36 GMT"
## Функція для видалення куки ##
Функція "видаляє" куки з браузера за допомогою установки терміну зберігання на одну секунду раніше поточного значення часу.
    
    Cookies.remove('nameCookie');
Видалення cookie із зазначенням атрибута path:
    
    Cookies.set ('name', 'value', {path: ''});
    Cookies.remove ('name', {path: '/'}); // не вийде (неправильний шлях)
    Cookies.remove ('name', {path: ''}); // видалиться (вказано правильний шлях)
 ## Встановлення атрибутів cookies ##
 Передача атрибутів при встановлені куків здійснюється за допомогою вказівки їх в якості значення останнього (3) аргументу функції Cookies.set.
 
    Cookies.set('name', 'value', { 
    expires: 365,
    path: '/news/',
    domain: 'm.mydomain.ru',
    secure: true
    }); // => "name=value; path=/news/; expires=Wed, 14 Mar 2018 12:54:28 GMT; domain=m.mydomain.ru; secure"
- **expires** - вказує тривалість зберігання cookie в браузері. Значення можна задавати як в форматі числа (кількість днів від поточного моменту часу), так і у вигляді дати. Якщо даний параметр не вказати, то cookie буде ссесіонним, тобто він віддалиться після закриття браузера.
- **path** - рядок, який вказує шлях.
- **domain** - рядок, який вказує домен. При цьому куки також будуть доступні у всіх піддоменів цього домену.
- **secure** - встановлює, чи потрібно при передачі cookies використовувати безпечний протокол (https). За замовчуванням: false, тобто не потрібно.
## Приклад використання ##
В даному прикладі зроблена сторінка, яка запитує Ваше ім'я при першому відвідуванні, потім вона зберігає Ваше ім'я в куки і показує його при наступних відвідинах.
![1](https://github.com/Kristina200220/ipz/blob/main/imag/1.png)
![2](https://github.com/Kristina200220/ipz/blob/main/imag/2.png)
Для куки задаємо термін зберігання в 1 рік від поточної дати, це означає, що браузер збереже Ваше ім'я навіть якщо Ви закриєте його.

Ви можете видалити кук натиснувши на посилання "Забути про мене", яка викликає функцію delete_cookie () і оновлює сторінку, щоб знову запитати у Вас ім'я.

Наводжу основну частину коду:
     
     function set_cookie ( name, value, expires_year, expires_month, expires_day, path, domain, secure )
       {
       
      var cookie_string = name + "=" + escape ( value );
    
      if ( expires_year )
      {
        var expires = new Date ( expires_year, expires_month, expires_day );
        cookie_string += "; expires=" + expires.toGMTString();
      }
    
      if ( path )
        cookie_string += "; path=" + escape ( path );
    
      if ( domain )
        cookie_string += "; domain=" + escape ( domain );
    
      if ( secure )
        cookie_string += "; secure";
    
      document.cookie = cookie_string;
    
    }
    
    function delete_cookie ( cookie_name )
    {
      var cookie_date = new Date ( );  //Поточна дата і час
      cookie_date.setTime ( cookie_date.getTime() - 1 );
      document.cookie = cookie_name += "=; expires=" + cookie_date.toGMTString();
    }
    
    function get_cookie ( cookie_name )
    {
      var results = document.cookie.match ( '(^|;) ?' + cookie_name + '=([^;]*)(;|$)' );
    
      if ( results )
        return ( unescape ( results[2] ) );
      else
        return null;
    }
    
    if ( ! get_cookie ( "username" ) )
    {
      var username = prompt ( "Напиши своє ім'я", "" );
    
      if ( username )
      {
        var current_date = new Date;
        var cookie_year = current_date.getFullYear ( ) + 1;
        var cookie_month = current_date.getMonth ( );
        var cookie_day = current_date.getDate ( );
        set_cookie ( "username", username, cookie_year, cookie_month, cookie_day );
        document.location.reload( );
      }
    }
    else
    {
      var username = get_cookie ( "username" );
      document.write ( "Привіт, " + username + ", рада бачити тебе на своєму сайті!" );
      document.write ( "<br><a href=\"javascript:delete_cookie('username'); document.location.reload( );\">Забути мене</a>" );
    }
# Локальна пам'ять #
   Локальна пам'ять - дані без терміну придатності, які зберігатимуться після закриття вікна браузера.
   
Це корисно для збереження даних, таких як налаштування користувача (світла або темна кольорова тема на веб-сайті), запам’ятовування товарів кошика для покупок або запам’ятовування користувача, який увійшов на веб-сайт.
# Методи #
Огляд методів localStorage.

## Додавання ключа та значення до локального сховища ##
setItem()

    localStorage.setItem('key', 'value')
    
    localStorage.setItem("name", "Kristina");
Ця функція приймає два параметри. Перший параметр - це ім'я, а другий параметр - значення.
## Отримати значення за ключем ##
Якщо ви хочете отримати значення для певного ключа, ви будете використовувати метод getItem ().
     
     localStorage.getItem('key')
     
     localStorage.getItem("name"); //Kristina
## Видалення ##
Ви можете видалити по ключу дані за допомогою removeItem ().

    localStorage.removeItem('key')
    
    localStorage.removeItem('name');
Використання clear () очистить усі локальні сховища.

    localStorage.clear()
 ## Приклад використання ##
 
 ![3](https://github.com/Kristina200220/ipz/blob/main/imag/3.PNG)
В полі для вводу можна писати якісь задачі і вони будуть відображатися нижче
Щоб очистити всі задачі треба натиснути кнопку Clear all

 ![4](https://github.com/Kristina200220/ipz/blob/main/imag/4.PNG)
Навожу шматок коду, написаного на JS

    const form = document.querySelector('form');
    const ul = document.querySelector('ul');
    const button = document.querySelector('button');
    const input = document.getElementById('item');
    let itemsArray = localStorage.getItem('items') ? JSON.parse(localStorage.getItem('items')) : [];

    localStorage.setItem('items', JSON.stringify(itemsArray));
    const data = JSON.parse(localStorage.getItem('items'));

    const liMaker = (text) => {
    const li = document.createElement('li');
    li.textContent = text;
    ul.appendChild(li);
    }

    form.addEventListener('submit', function (e) {
    e.preventDefault();

    itemsArray.push(input.value);
    localStorage.setItem('items', JSON.stringify(itemsArray));
    liMaker(input.value);
    input.value = "";
    });

    data.forEach(item => {
    liMaker(item);
    });

    button.addEventListener('click', function () {
    localStorage.clear();
    while (ul.firstChild) {
    ul.removeChild(ul.firstChild);
     }
    itemsArray = [];
    });
# Висновок #
Оскільки між різними методами зберігання є незначна різниця, в більшості випадків завжди використовується локальне сховище. Але якщо вам потрібен доступ до даних на сервері, тоді файли cookie корисні.
