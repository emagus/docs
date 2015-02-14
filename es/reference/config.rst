Leyendo Configuraciones
=======================
:doc:`Phalcon\\Config <../api/Phalcon_Config>` este componente se usa para leer ficheros de varios formatos (usando adaptadores)
y crearlos como objetos PHP para usarlos en una aplicación

File Adapters
-------------
Los adaptadores disponibles son:

+-----------+---------------------------------------------------------------------------------------------------------+
| Tipo file | Descripcion                                                                                             |
+===========+=========================================================================================================+
| Ini       | Usa ficheros INI para almacenar la configuración. Internamente usa la efnción de PHP parse_ini_file.    |
+-----------+---------------------------------------------------------------------------------------------------------+
| Array     |Utiliza arrays multidimiensionales de PHP para almacenar la configuración.                              |
|           |Este adpatador ofrece mejor rendimiento.                                                                 | 
+-----------+---------------------------------------------------------------------------------------------------------+

Arrays Nativos
--------------
El siguiente ejemplo muestra como convertir arrays en objetos Phalcon\\Config. Esta opción ofrece mejor rendimiento por no tener que leer ficheros durate la petición.

.. code-block:: php

    <?php

    $settings = array(
        "database" => array(
            "adapter"  => "Mysql",
            "host"     => "localhost",
            "username" => "scott",
            "password" => "cheetah",
            "name"     => "test_db",
        ),
         "app" => array(
            "controllersDir" => "../app/controllers/",
            "modelsDir"      => "../app/models/",
            "viewsDir"       => "../app/views/",
        ),
        "mysetting" => "the-value"
    );

    $config = new \Phalcon\Config($settings);

    echo $config->app->controllersDir, "\n";
    echo $config->database->username, "\n";
    echo $config->mysetting, "\n";

Si quieres organizar mejor tu proyecto, puedes salvar el array en otro fichero y leerlo.

.. code-block:: php

    <?php

    require "config/config.php";
    $config = new \Phalcon\Config($settings);

Leyendo ficheros INI
--------------------
Ficheros INI son habituales para almacenar la configuraión. Phalcon\\Config utiliza la función parse_ini_file de PHP para leer estos ficheors. Las secciones son parseadas en sub-configuraciones para hacerlas mas accesibles.

.. code-block:: ini

    [database]
    adapter  = Mysql
    host     = localhost
    username = scott
    password = cheetah
    name     = test_db

    [phalcon]
    controllersDir = "../app/controllers/"
    modelsDir      = "../app/models/"
    viewsDir       = "../app/views/"

    [models]
    metadata.adapter  = "Memory"

Tú puedes leer el fichero a continuación:

.. code-block:: php

    <?php

    $config = new \Phalcon\Config\Adapter\Ini("path/config.ini");

    echo $config->phalcon->controllersDir, "\n";
    echo $config->database->username, "\n";
    echo $config->models->metadata->adapter, "\n";

Fusionando Configuraciones
--------------------------
Phalcon\\Config permite fusionar un objeto de configuración dentro de otro recursivamente:

.. code-block:: php

    <?php

    $config = new \Phalcon\Config(array(
        'database' => array(
            'host' => 'localhost',
            'name' => 'test_db'
        ),
        'debug' => 1
    ));

    $config2 = new \Phalcon\Config(array(
        'database' => array(
            'username' => 'scott',
            'password' => 'secret',
        )
    ));

    $config->merge($config2);

    print_r($config);

El codigo escrito produce lo siguiente:

.. code-block:: html

    Phalcon\Config Object
    (
        [database] => Phalcon\Config Object
            (
                [host] => localhost
                [name] => test_db
                [username] => scott
                [password] => secret
            )
        [debug] => 1
    )

