Command Line Applications (CLI)
=========================

Las aplicaciones CLI son ejectuadas desde la líneas de comandos. Son muy últiles para crear tareas cron, scripts, comandos y utilidades entre otros.

Tasks
-----
Las Tasks son similares a los controladores, así pueden ser implementadas

.. code-block:: php

	<?php

	class MonitoringTask extends \Phalcon\CLI\Task
	{

	    public function mainAction()
	    {

	    }

	}

.. code-block:: php

	<?php

	//Usando el CLI en el contenedor de servicios inicial
	$di = new Phalcon\DI\FactoryDefault\CLI();

	//Crear una consola de aplicación
	$console = new \Phalcon\CLI\Console();
	$console->setDI($di);

	//
	$console->handle(array('shell_script_name', 'echo'));

