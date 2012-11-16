# How to use

    local.php

    'YUI' => array(
        'compressor' => array(
            'compress' => true, 
            'tmp' => "/../tmp",
            'jar' => "{PATH-TO-APP}/vendor/dwsla/minify/lib/YUI/yuicompressor-2.4.2.jar",
        ),
    ),
    
    
    module.config.php
    
    'controller_plugins' => array(
        'factories' => array(
        
            'compressor' => function($sm){

                // get the "real" service manager frrom the "controller plugin" service manager
                $serviceManager = $sm->getServiceLocator();

                // extract the config we need
                $config = $serviceManager->get('Config');
                $jarFile = $config['YUI']['compressor']['jar'];
                $tempDir = $config['YUI']['compressor']['tmp'];

                // inject into our plugin
                $plugin = new \YUI\YUICompressor\YUICompressor($jarFile,$tempDir);
                return $plugin;
            },
            
        ),
    ),
    
    
    MyController.php
    
    $this->compressor()->setOption('type', 'css');
    $this->compressor()->addString($css);
    $output = $this->compressor()->compress();
