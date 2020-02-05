### PHP Data Objects

PDO – PHP Data Object – унифицированный интерфейс для доступа к базам данных, своего рода ADO.NET в мире PHP.

```php
# Параметры подключения
$driver   = '{ODBC Driver 11 for SQL Server}';
$server   = '(local)';
$db       = 'irbis';
$user     = 'librarian';
$password = 'secret';
 
# Тонкая настройка подключения
$options = array(
    PDO::ATTR_ERRMODE            => PDO::ERRMODE_EXCEPTION,
    PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
    PDO::ATTR_EMULATE_PREPARES   => false
);
 
$dsn = "odbc:Driver=$driver;Server=$server;Database=$db;";
 
# Собственно подключение к базе данных
$connection = new PDO ($dsn, $user, $password, $options);
 
# Запрос к базе без параметров
$sql = 'SELECT TOP 10 [name], [category], [ticket] FROM [readers]';
$statement = $connection->query($sql);
while ($row = $statement->fetch()) {
    # каждая строка представлена ассоциативным массивом
    echo $row['name'], ', ', $row['category'], ', ', $row['ticket'], PHP_EOL;
}
 
# Получения скаляра
$sql = 'SELECT COUNT(*) from [readers]';
$statement = $connection->query($sql);
$count = $statement->fetchColumn();
echo "Всего читателей: $count\n";
 
# Получение в виде объекта
$sql = 'SELECT TOP 10 [name], [category], [ticket] FROM [readers]';
$statement = $connection->query($sql);
$readers = $statement->fetchAll(PDO::FETCH_OBJ);
foreach ($readers as $reader) {
    echo "Name    : $reader->name\n";
    echo "Category: $reader->category\n";
    echo "Ticket  : $reader->ticket\n";
}
 
# Получение в виде хеш-таблицы 'номер билета => ФИО'
$sql = 'SELECT TOP 10 [ticket], [name] FROM [readers]';
$statement = $connection->query($sql);
$readers = $statement->fetchAll(PDO::FETCH_KEY_PAIR);
echo $readers['996'], PHP_EOL;
echo $readers['2397'], PHP_EOL;
echo $readers['2495'], PHP_EOL;
 
# Запрос к базе с параметрами
$sql = "SELECT TOP 10 [name], [category] FROM [readers] WHERE [name] LIKE :family";
$statement = $connection->prepare($sql);
$family = mb_convert_encoding('Иванов%', 'CP1251', 'UTF-8');
$statement->execute(array('family' => $family));
foreach ($statement as $row) {
    echo $row['name'], ', ', $row['category'], ', ', PHP_EOL;
}
 
# Отключение от сервера
unset($connection);
```