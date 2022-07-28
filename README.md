twando
======

Monitor your followers, auto follow and unfollow, auto DM new followers, schedule tweets, search &amp; mass follow new users and much, much more.


Requirements
======

PHP 5.2+
MySQL 5+
cURL
OpenSSL
Cron Jobs (Remote cron job services are supported also)


Installation
======

Installing Twando is bascially a 3 minute job, but there are some more advanced configuration options which are explained in detail below. 

3 Minute Install
Download the latest version of Twando.
Unzip the zip file and rename inc/config-sample.php to inc/config.php.
Update the options in config.php with your required settings.
Upload the entire contents of the folder to a directory on your server, e.g. http://www.yoursite.com/twando/.
Visit http://www.yoursite.com/twando/install_tables.php in your browser.
That's pretty much it! You can now set up your Twitter application and auth accounts by following the instructions at http://www.yoursite.com/twando/.
Upgrading From a Previous Version

Upload the entire contents of the download zip to where you previously installed Twando on your server.
Delete the inc/config-sample.php file from your server.
That's it; sorry if you wanted more!
Detailed Configuration Options

In inc/config.php, there are several options you can configure before installing Twando:
DB_NAME
The name of the MySQL database you will use with Twando
DB_USER
The username of the MySQL user you will use with Twando. This user must have full privileges on the database specified above.
DB_PASSWORD
The password for the MySQL username specified above.
DB_HOST
The host address of your MySQL database. This will usually be "localhost".
DB_PREFIX
All created MySQL tables will be prefixed with this value. This gives you the option to run multiple Twando installs from a single database if you wish. You shouldn't set this to anything longer than 10 characters. Changing this after you install will break the script.
LOGIN_USER
The username you will use to log in to your Twando install.
LOGIN_PASSWORD
The password you will use to log in to your Twando install. Try to use a password that's long and hard to guess.
CRON_KEY
This should be a hard to guess string of characters. This is checked when the cron job files are called; since Twando supports remote http calls to your cron jobs, having this key prevents unauthorised running of your cron jobs.
TWANDO_LANG
Twando has been built to support multiple languages in future; currently this should be left as "english".
TIMESTAMP_FORMAT
The format the time and date should be displayed in at various parts of the script, using the parameters from PHP's date() function.
BASE_LINK_URL
The URL of your install, e.g. http://www.yoursite.com/twando/. If you don't set this, the script will try and work out what it is for you but it can't handle things like port numbers in your URL and other fancy stuff like that. If you set this incorrectly, the script won't work.
Random Password Values

You can use the values below in your config.php value. These are randomly generated; refresh the page for new values: 

define('LOGIN_USER','vY1iQ9N9Dp');
define('LOGIN_PASSWORD','Onh2i6qQaSQO99X');

define('CRON_KEY','N04FI9g1fSRRGUsj3S0LzmPHI');



MANUAL
======


This manual assumes you have already installed Twando. The below combined with the text help found within your actual Twando installation covers pretty much everything you'll ever need to know. 

Contents
Registering Your Application
Authorizing a Twitter Account
Setting Up Cron Jobs
Follow / Unfollow Settings
Tweet Settings
Log Settings
Multi Account Functions
Registering Your Application

Once installed, the first step you must complete is to register your Twando application with Twitter. The homepage of your install will guide you through this process in full, complete with screenshots of the required steps. It's really very easy to do and once complete you will be able to enter your Consumer Key and Consumer Secret in the text boxes provided. You can edit these values at any time from the homepage of your install if required. Top tip: Only read and write access is required in your Twitter application settings to use Twando (this access level allows the sending of DMs also). 

Authorizing a Twitter Account
======

You can authorize as many Twitter accounts as you like with your application; simply click the large "Sign in with Twitter" button. Top tip: If you are signed into a Twitter account that has already been authorized, Twitter will simply re-auth your application for that account (it won't give you the option to sign out of Twitter first). Therefore, when authorizing a new account, you should always ensure you are either signed out of all Twitter accounts on the Twitter website, or signed into the account you wish to authorize. 

Setting Up Cron Jobs

After you have authorized your Twitter accounts, you should set up the cron jobs as soon as possible so this is taken care of. There are two cron jobs that need to be set up; full instructions on how to set these up can be found by clicking the "Cron job instructions" link towards the bottom of the homepage of your install. The cron_follow.php script logs who has followed and unfollowed your accounts, as well as taking care of the auto follow/unfollow and auto DM functionality. The script is designed to throttle requests for accounts with a large number of friends and followers. With the Twitter API 1.0, processing around 1.7 million friends and followers an hour was no problem. Twitter API 1.1 has reduced the limits significantly; if you have less than about 250,000 friends or followers, you should be comfortably able to run this cron job once per hour, otherwise it's recommended to run it once per day. 

The cron_tweet.php script powers the scheduled tweets functionality. Any tweets posted for a time equal to or less than the current time will be posted when the cron job is run, so how often you run this cron job depends on how many tweets you have to post and how close to the time you specify you need to post them. Normally running this cron job every five minutes would be reasonable. It's always preferable to run cron jobs locally from your own hosting account, but if for some reason your host doesn't support this, you can also call the cron jobs using a remote cron job service (just search on Google if you need one; there are loads of them about). 

It is not recommended to run the cron scripts directly via a browser as they can take several minutes to complete; for large accounts they could take in excess of one hour so make sure you set up cron jobs to run these scripts. 

Follow / Unfollow Settings

To access the follow / unfollow settings for an authorized Twitter account, simply click the "Follows" link in the account table on the homepage of your install. There are then several tabbed options available which are detailed below. 

Auto Follow Settings
======

There are several checkbox options under this tab:
Auto follow back all followers / new followers of this account
When ticked, Twando will automatically follow back your followers for this account when the follow cron job is run. Selecting "all followers" from the drop down means that all followers of this account will be followed back. Selecting "new followers" means that only new followers will be followed back. This can be very useful if you have a positive difference between your followers and who you are following and want to maintain this by only following back new followers.
Auto unfollow users who are not following you
If ticked, Twando will check who you are following against who is following you and automatically unfollow anyone who's not following you when the follow cron job is run.
Auto DM users when you follow them back
If you want to send an Auto DM to your followers (when you automatically follow them back), tick this box. The DM will only be sent if you have set one in the "Auto DM Message" tab (detailed below). Of course, many people find this super annoying, but the option is there if you want it.

Unfollow Exclusions
======

Twando allows you to specify unfollow exclusions; these come into play if you are using the "Auto unfollow users who are not following you" option detailed above. Any Twitter accounts you specify here will not be unfollowed automatically; this is really useful for example if you run Twando on your main personal Twitter account, but there are certain people you follow that you don't want to automatically unfollow. You can add as many exclusions as you require and then delete them later if you wish. When adding an exclusion, there is a checkbox titled "Follow these users on Twitter now?" which as the name suggests will follow the users you submit on Twitter. You can therefore use this screen to mass follow a list of Twitter users; there is even an additional checkbox which appears so you can mass follow on Twitter without needing to add the users to your exclusion list. 

Follow Exclusions
======

This works in exactly the same way as "Unfollow Exclusions" except users specifed here will not be followed back when they follow you and you have enabled "Auto follow back". 

Auto DM Message
======

If you have enabled "Auto DM users when you follow them back" then you can specify the message you will automatically DM to new followers here. To limit the annoyingness of this feature for your followers, the DM is sent when you follow back (not when they follow you), so at least the follower then has the option to reply to you if they wish. 

Search to Follow
======

Twando allows you to search for new users to follow and mass follow these users with two clicks. Top tip: Mass following too quickly is a very easy way to get your Twitter account banned.
Tweet Based Search
This method searches for users who have tweeted about a particular topic; for example searching for "Eden Hazard" would display users who posted about this.
User Based Search
This method is slightly different in that it searches user data (screen name, full name, description etc); searching here for "Eden Hazard" would bring up his official account as well as the various parody accounts using his name and so on.
Once the search is complete, tick the checkbox underneath the users you want to follow (there is a "Select all users" link above the results to select them all) and click the "Follow Selected Users" button to follow the selected users. Top tip: When following new users, you should untick the "Auto unfollow users who are not following you" from the "Auto Follow Settings" tab; a possible approach might be to disable auto unfollowing, follow some users and then re-enable auto unfollow after a week or so. 

Tweet Settings
======

To access the tweet settings for an authorized Twitter account, simply click the "Tweets" link in the account table on the homepage of your install. There are then several tabbed options available which are detailed below. 

Post Quick Tweet
======

Although Twando is certainly not intended as a replacement for your Twitter client of choice, or even the Twitter web interface, the ability to simply post a quick tweet from your account is included. This is also useful for checking your account is working as expected. 

Scheduled Tweets
======

This screen lists all the tweets currently scheduled for the selected account. You can edit any scheduled tweet by clicking the "Edit" link; click the bin (trashcan) icon to delete a scheduled tweet. 

Schedule a Tweet
======

Here you can schedule a tweet to be posted from the selected Twitter account. The time and date must be specified in MySQL format (YYYY-MM-DD HH:MM) but this is easily set by clicking the calendar icon which will bring up a time and date selector window. Tweets are posted based on the PHP time set in your hosting account; this may not be the same as your local time. The scheduled tweet cron posts scheduled tweets from before or exactly the time the cron is run, so the post time may not be exactly the same as the time you specify depending on how often you run the tweet cron job. 

Bulk CSV Tweet Upload
======

This system allows you to upload a CSV of scheduled tweets. The CSV should contain just two columns; column 1 should contain the tweet time in MySQL format (YYYY-MM-DD HH:MM), column 2 the tweet text. There is an example csv included with your install. The maximum size of the CSV you can upload depends on your host; typically shared hosts limit file uploads to 2mb but you may be able to upload larger files. Top tip: Spreadsheet programs such as Microsoft Excel will sometimes convert the date into an incorrect format; it's therefore recommended to create your csv in a text editor such as Notepad if you are doing this manually. 

Log Settings
======

To access the log settings for an authorized Twitter account, simply click the "Logs" link in the account table on the homepage of your install. There are then several tabbed options available which are detailed below. 

Log Settings
======

This checkbox simple sets if you would like to log the actions for this account when either the follow or tweet cron job is run. By default this is enabled when you authorize a new account. We would recommend enabling logging for all accounts as the logs provide useful information. 

View Follow Logs
======

This table displays the logs from the follow cron job. The information here shows who has unfollowed your account, who you have followed and consequently which users Twando auto followed or unfollowed for you. This screen also logs when automatic DMs are sent. In the "Affected Users" column you will see the Twitter profile images of the relevant users affected by a particular log entry. Hovering over the image will show additional useful information about their account (name, screen name, follower counts etc); clicking the image will open a new window link to their profile on Twitter.com. 

View Tweet Logs
======

This table shows your scheduled tweet history and shows both the time the tweet was posted and also whether or not the tweet posting was sucessful. 

Purge Log History
======

As the name suggests, this tab allows you to delete log history for a particular account if you wish. There is also an option to "Empty user cache"; the user cache is shared between all your Twitter accounts and is used to display more information about Twitter users in the "View Follow Logs" section. If one of your accounts is being followed by 1000 new users a day, the cache could get quite large quite quickly so you may want to empty it periodically. If you are proficient with phpMyAdmin, you can truncate the (tw_)user_cache table yourself at any time. This will not affect the log records; it just means that the additional information about a user (name, screen name, follower counts etc) will not be displayed in the "Affected Users" column in the "View Follow Logs" section. 

Multi Account Functions
======

Once you have authorized two or more Twitter accounts, you can then make use of the multi account functions Twando provides. These can be accessed by clicking the "Multi account functions" link towards the bottom of the homepage of your install. There are then several tabbed options available which are detailed below. 

Cross Follow Accounts
======

This option is extremely useful if you have a lot of Twitter accounts authorized with your Twando install and want to increase the number of followers to all of them. With one click, Twando will follow all your Twitter accounts from all your other Twitter accounts. You can also do the reverse and cross unfollow to remove the connection between accounts. 

All Follow / Unfollow
======

This section allows you to follow or unfollow a list of screen names or Twitter id's from all your accounts. For example, if you had 100 Twitter accounts authorized and wanted them all to follow a particular user, you could perform this operation very easily here. 

Multi Tweet
======

As the name suggests, you can enter a Tweet here which will be posted by all your Twitter accounts simultaneously.

====================================================================================================================================
====================================================================================================================================
====================================================================================================================================

Твандо
Следите за своими подписчиками, автоматически подписывайтесь и отписывайтесь, автоматически отмечайте новых подписчиков в DM, планируйте твиты, ищите и подписывайтесь на новых пользователей и многое, многое другое.

Требования
PHP 5.2+ MySQL 5+ cURL OpenSSL Cron Jobs (также поддерживаются службы удаленных заданий cron)

Монтаж
Установка Twando в основном занимает 3 минуты, но есть некоторые дополнительные параметры конфигурации, которые подробно описаны ниже.

Установка за 3 минуты Загрузите последнюю версию Twando. Разархивируйте zip-файл и переименуйте inc/config-sample.php в inc/config.php. Обновите параметры в config.php необходимыми настройками. Загрузите все содержимое папки в каталог на вашем сервере, например. http://www.yoursite.com/twando/. Посетите http://www.yoursite.com/twando/install_tables.php в своем браузере. Вот и все! Теперь вы можете настроить приложение Twitter и учетные записи авторизации, следуя инструкциям на странице http://www.yoursite.com/twando/. Обновление предыдущей версии

Загрузите все содержимое ZIP-файла для загрузки туда, где вы ранее установили Twando на своем сервере. Удалите файл inc/config-sample.php с вашего сервера. Вот и все; извините, если вы хотели больше! Подробные параметры конфигурации

В inc/config.php есть несколько опций, которые вы можете настроить перед установкой Twando: DB_NAME Имя базы данных MySQL, которую вы будете использовать с Twando DB_USER Имя пользователя MySQL, которого вы будете использовать с Twando. Этот пользователь должен иметь полные права доступа к базе данных, указанной выше. DB_PASSWORD Пароль для указанного выше имени пользователя MySQL. DB_HOST Адрес хоста вашей базы данных MySQL. Обычно это будет «localhost». DB_PREFIX Все созданные таблицы MySQL будут иметь префикс с этим значением. Это дает вам возможность запустить несколько установок Twando из одной базы данных, если хотите. Вы не должны устанавливать это значение длиннее 10 символов. Изменение этого после установки приведет к поломке скрипта. LOGIN_USER Имя пользователя, которое вы будете использовать для входа в вашу установку Twando. LOGIN_PASSWORD Пароль, который вы будете использовать для входа в вашу установку Twando. Попробуйте использовать длинный пароль, который трудно подобрать. CRON_KEY Это должна быть трудно угадываемая строка символов. Это проверяется при вызове файлов заданий cron; поскольку Twando поддерживает удаленные HTTP-вызовы к вашим заданиям cron, наличие этого ключа предотвращает несанкционированный запуск ваших заданий cron. TWANDO_LANG Twando поддерживает несколько языков в будущем; в настоящее время это должно быть оставлено как «английский». TIMESTAMP_FORMAT Формат, в котором время и дата должны отображаться в различных частях скрипта, с использованием параметров PHP-функции date(). BASE_LINK_URL URL-адрес вашей установки, например. http://www.yoursite.com/twando/. Если вы не установите это значение, скрипт попытается понять, что он для вас делает, но он не сможет обрабатывать такие вещи, как номера портов в вашем URL-адресе и другие подобные причудливые вещи. Если вы установите это неправильно, скрипт не будет работать. Случайные значения пароля

Вы можете использовать приведенные ниже значения в своем значении config.php. Они генерируются случайным образом; обновить страницу для новых значений:

define('LOGIN_USER','vY1iQ9N9Dp'); define('LOGIN_PASSWORD','Onh2i6qQaSQO99X');

define('CRON_KEY','N04FI9g1fSRRGUsj3S0LzmPHI');

РУКОВОДСТВО
В этом руководстве предполагается, что вы уже установили Twando. Приведенное ниже в сочетании с текстовой справкой, найденной в вашей фактической установке Twando, охватывает почти все, что вам когда-либо понадобится знать.

Содержание Регистрация приложения Авторизация учетной записи Twitter Настройка заданий Cron Настройки подписки/отписки Настройки твита Настройки журнала Функции нескольких учетных записей Регистрация приложения

После установки первым шагом, который вы должны выполнить, является регистрация вашего приложения Twando в Twitter. Домашняя страница вашей установки полностью проведет вас через этот процесс со скриншотами необходимых шагов. Это действительно очень легко сделать, и после завершения вы сможете ввести свой ключ потребителя и секрет потребителя в предоставленные текстовые поля. Вы можете изменить эти значения в любое время с домашней страницы вашей установки, если это необходимо. Главный совет: для использования Twando в настройках вашего приложения Twitter требуется доступ только для чтения и записи (этот уровень доступа также позволяет отправлять DM).

Авторизация учетной записи Twitter
Вы можете авторизовать любое количество учетных записей Twitter с помощью своего приложения; просто нажмите большую кнопку «Войти через Twitter». Главный совет: если вы вошли в учетную запись Twitter, которая уже была авторизована, Twitter просто повторно аутентифицирует ваше приложение для этой учетной записи (это не даст вам возможность сначала выйти из Twitter). Поэтому при авторизации новой учетной записи вы всегда должны убедиться, что вы либо вышли из всех учетных записей Twitter на веб-сайте Twitter, либо вошли в учетную запись, которую хотите авторизовать.

Настройка заданий Cron

После того, как вы авторизуете свои учетные записи Twitter, вы должны как можно скорее настроить задания cron, чтобы об этом позаботились. Необходимо настроить два задания cron; полные инструкции по их настройке можно найти, щелкнув ссылку «Инструкции по работе с Cron» в нижней части домашней страницы вашей установки. Скрипт cron_follow.php регистрирует, кто подписался на ваши аккаунты и отписался от них, а также позаботится о функциях автоматического подписки/отписки и автоматического DM. Скрипт предназначен для ограничения запросов на учетные записи с большим количеством друзей и подписчиков. С Twitter API 1.0 не было проблем с обработкой около 1,7 миллиона друзей и подписчиков в час. Twitter API 1.1 значительно уменьшил ограничения; если у вас менее 250 000 друзей или подписчиков, вы должны иметь возможность запускать это задание cron один раз в час, в противном случае рекомендуется запускать его один раз в день.

Сценарий cron_tweet.php обеспечивает функциональность запланированных твитов. Любые твиты, опубликованные в течение времени, равного или меньшего текущего времени, будут опубликованы при запуске задания cron, поэтому частота запуска этого задания cron зависит от того, сколько твитов вы должны опубликовать и насколько близко ко времени, которое вы укажете. нужно их опубликовать. Обычно запуск этого задания cron каждые пять минут был бы разумным. Всегда предпочтительнее запускать задания cron локально из вашей собственной учетной записи хостинга, но если по какой-то причине ваш хост не поддерживает это, вы также можете вызывать задания cron с помощью службы удаленных заданий cron (просто выполните поиск в Google, если вам это нужно; их полно вокруг).

Не рекомендуется запускать cron-скрипты напрямую через браузер, так как их выполнение может занять несколько минут; для больших учетных записей это может занять более одного часа, поэтому убедитесь, что вы настроили задания cron для запуска этих сценариев.

Настройки подписки/отписки

Чтобы получить доступ к настройкам подписки/отписки для авторизованной учетной записи Twitter, просто щелкните ссылку «Подписаться» в таблице учетных записей на главной странице вашей установки. Затем доступно несколько вариантов с вкладками, которые подробно описаны ниже.

Настройки автоматического следования
На этой вкладке есть несколько флажков: Автоматически следить за всеми подписчиками / новыми подписчиками этой учетной записи. Если этот флажок установлен, Twando будет автоматически следить за вашими подписчиками для этой учетной записи при запуске задания cron. Выбор «всех подписчиков» в раскрывающемся списке означает, что все подписчики этой учетной записи будут подписаны в ответ. Выбор «новых подписчиков» означает, что только новые подписчики будут отслеживаться. Это может быть очень полезно, если у вас есть положительная разница между вашими подписчиками и теми, за кем вы следите, и вы хотите сохранить это, подписываясь только на новых подписчиков. Автоматически отменять подписку на пользователей, которые не подписаны на вас Если отмечено, Twando будет сравнивать тех, на кого вы подписаны, и тех, кто подписан на вас, и автоматически отменять подписку на всех, кто не подписан на вас, при запуске задания cron. Автоматически отправлять сообщения пользователям, когда вы подписываетесь на них в ответ. Если вы хотите отправлять автоматические сообщения своим подписчикам (когда вы автоматически подписываетесь на них в ответ), установите этот флажок. DM будет отправлено только в том случае, если вы установили его на вкладке «Автоматическое сообщение DM» (подробности ниже). Конечно, многие люди находят это очень раздражающим, но вариант есть, если вы этого хотите.

Отписаться от исключений
Twando позволяет указать исключения для отписки; они вступают в игру, если вы используете опцию «Автоматическая отмена подписки на пользователей, которые не подписаны на вас», описанную выше. Любые учетные записи Twitter, которые вы укажете здесь, не будут автоматически отписаны; это действительно полезно, например, если вы запускаете Twando в своей основной личной учетной записи Twitter, но есть определенные люди, на которых вы подписаны, и вы не хотите автоматически отписываться от них. Вы можете добавить столько исключений, сколько вам нужно, а затем удалить их позже, если хотите. При добавлении исключения есть флажок «Подписаться на этих пользователей в Твиттере сейчас?» который, как следует из названия, будет следовать за пользователями, которых вы отправляете в Twitter. Таким образом, вы можете использовать этот экран для массового подписки на список пользователей Twitter; есть даже дополнительный флажок, который появляется, чтобы вы могли массово следить за Twitter без необходимости добавлять пользователей в свой список исключений.

Следовать за исключениями
Это работает точно так же, как «Отменить подписку», за исключением того, что пользователи, указанные здесь, не будут отслеживаться, когда они подписываются на вас, и вы включили «Автоматическое отслеживание».

Автоматическое сообщение DM
Если вы включили «Автоматически отправлять сообщения пользователям, когда вы подписываетесь на них в ответ», вы можете указать здесь сообщение, которое вы будете автоматически отправлять новым подписчикам. Чтобы уменьшить раздражающую способность этой функции для ваших подписчиков, DM отправляется, когда вы подписываетесь назад (а не когда они следуют за вами), поэтому, по крайней мере, у подписчика есть возможность ответить вам, если они того пожелают.

Поиск, чтобы подписаться
Twando позволяет вам искать новых пользователей и массово подписываться на них двумя щелчками мыши. Главный совет: слишком быстрая массовая подписка — это очень простой способ заблокировать ваш аккаунт в Твиттере. Поиск на основе твитов Этот метод ищет пользователей, которые писали в Твиттере на определенную тему; например, при поиске «Эден Хазард» будут отображаться пользователи, опубликовавшие об этом. Поиск по пользователям Этот метод немного отличается тем, что он ищет данные пользователя (экранное имя, полное имя, описание и т. д.); поиск здесь «Эден Хазард» приведет к его официальному аккаунту, а также к различным пародийным аккаунтам, использующим его имя, и так далее. После завершения поиска установите флажок под пользователями, за которыми вы хотите следить (для выбора всех пользователей есть ссылка «Выбрать всех пользователей» над результатами) и нажмите кнопку «Подписаться на выбранных пользователей», чтобы подписаться на выбранных пользователей. Главный совет: когда вы подписываетесь на новых пользователей, вы должны снять флажок «Автоматическая отмена подписки на пользователей, которые не подписаны на вас» на вкладке «Настройки автоматического подписки»; возможный подход может состоять в том, чтобы отключить автоматическую отписку, подписаться на некоторых пользователей, а затем снова включить автоматическую отписку примерно через неделю.

Настройки твита
Чтобы получить доступ к настройкам твитов для авторизованной учетной записи Twitter, просто щелкните ссылку «Твиты» в таблице учетных записей на главной странице вашей установки. Затем доступно несколько вариантов с вкладками, которые подробно описаны ниже.

Опубликовать быстрый твит
Хотя Twando, безусловно, не предназначен для замены выбранного вами клиента Twitter или даже веб-интерфейса Twitter, в него включена возможность просто опубликовать быстрый твит из вашей учетной записи. Это также полезно для проверки того, что ваша учетная запись работает должным образом.

Запланированные твиты
На этом экране перечислены все твиты, которые в настоящее время запланированы для выбранной учетной записи. Вы можете отредактировать любой запланированный твит, щелкнув ссылку «Редактировать»; щелкните значок корзины (корзины), чтобы удалить запланированный твит.

Запланировать твит
Здесь вы можете запланировать публикацию твита из выбранной учетной записи Twitter. Время и дата должны быть указаны в формате MySQL (ГГГГ-ММ-ДД ЧЧ:ММ), но это легко установить, щелкнув значок календаря, который откроет окно выбора времени и даты. Твиты публикуются на основе времени PHP, установленного в вашей учетной записи хостинга; это может не совпадать с вашим местным временем. Запланированный твит cron публикует запланированные твиты до или точно во время запуска cron, поэтому время публикации может не совпадать с указанным вами временем в зависимости от того, как часто вы запускаете задание твита cron.

Массовая загрузка твитов в формате CSV
Эта система позволяет загружать запланированные твиты в формате CSV. CSV должен содержать только два столбца; столбец 1 должен содержать время твита в формате MySQL (ГГГГ-ММ-ДД ЧЧ:ММ), столбец 2 — текст твита. В вашу установку включен пример csv. Максимальный размер CSV, который вы можете загрузить, зависит от вашего хоста; обычно общие хосты ограничивают загрузку файлов до 2 МБ, но вы можете загружать файлы большего размера. Главный совет: программы для работы с электронными таблицами, такие как Microsoft Excel, иногда преобразуют дату в неправильный формат; поэтому рекомендуется создавать CSV-файл в текстовом редакторе, таком как Блокнот, если вы делаете это вручную.

Настройки журнала
Чтобы получить доступ к настройкам журнала для авторизованной учетной записи Twitter, просто щелкните ссылку «Журналы» в таблице учетных записей на главной странице вашей установки. Затем доступно несколько вариантов с вкладками, которые подробно описаны ниже.

Настройки журнала
Этот простой флажок устанавливает, хотите ли вы регистрировать действия для этой учетной записи, когда выполняется задание cron подписки или твита. По умолчанию это включено при авторизации новой учетной записи. Мы рекомендуем включить ведение журнала для всех учетных записей, поскольку журналы предоставляют полезную информацию.

Просмотр журналов подписки
В этой таблице отображаются журналы следующего задания cron. Информация здесь показывает, кто отписался от вашей учетной записи, за кем вы подписались и, следовательно, какие пользователи Twando автоматически подписались или отписались от вас. Этот экран также регистрирует отправку автоматических DM. В столбце «Затронутые пользователи» вы увидите изображения профилей Twitter соответствующих пользователей, затронутых определенной записью в журнале. Наведение курсора на изображение покажет дополнительную полезную информацию об их учетной записи (имя, псевдоним, количество подписчиков и т. д.); щелчок по изображению откроет в новом окне ссылку на их профиль на Twitter.com.

Просмотр журналов твитов
В этой таблице показана ваша запланированная история твитов, а также время публикации твита, а также то, была ли публикация твита успешной.

Очистить историю журнала
Как следует из названия, эта вкладка позволяет при желании удалить историю журнала для определенной учетной записи. Также есть опция «Очистить кеш пользователя»; пользовательский кеш распределяется между всеми вашими учетными записями Twitter и используется для отображения дополнительной информации о пользователях Twitter в разделе «Просмотр журналов подписки». Если за одной из ваших учетных записей следят 1000 новых пользователей в день, кеш может довольно быстро стать довольно большим, поэтому вы можете периодически очищать его. Если вы хорошо разбираетесь в phpMyAdmin, вы можете самостоятельно обрезать таблицу (tw_)user_cache в любое время. Это не повлияет на записи журнала; это просто означает, что дополнительная информация о пользователе (имя, псевдоним, количество подписчиков и т. д.) не будет отображаться в столбце «Затронутые пользователи» в разделе «Просмотр журналов подписки».

Функции нескольких учетных записей
После авторизации двух или более учетных записей Twitter вы можете использовать функции нескольких учетных записей, которые предоставляет Twando. Доступ к ним можно получить, щелкнув ссылку «Функции нескольких учетных записей» в нижней части главной страницы вашей установки. Затем доступно несколько вариантов с вкладками, которые подробно описаны ниже.

Перекрестное отслеживание учетных записей
Эта опция чрезвычайно полезна, если у вас есть много учетных записей Twitter, авторизованных при установке Twando, и вы хотите увеличить количество подписчиков для всех из них. Одним щелчком мыши Twando будет следить за всеми вашими учетными записями в Твиттере со всех других ваших учетных записей в Твиттере. Вы также можете сделать обратное и перекрестно отписаться, чтобы удалить связь между учетными записями.

Все подписаться / отменить подписку
Этот раздел позволяет вам подписаться или отписаться от списка псевдонимов или идентификаторов Twitter из всех ваших учетных записей. Например, если у вас было авторизовано 100 учетных записей Twitter и вы хотели, чтобы все они были подписаны на определенного пользователя, вы можете очень легко выполнить эту операцию здесь.

Мульти твит
Как следует из названия, здесь вы можете ввести твит, который будет опубликован всеми вашими учетными записями в Твиттере одновременно.
