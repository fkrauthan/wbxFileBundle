wbxFileBundle
=============

a Symfony2 Bundle to handle file upload


Installation
============

Bring in the vendor libraries
-----------------------------

This can be done in two different ways:

**Method #1**) Use composer

    "require": {
        "wbx/file-bundle": "*"
    }
    
    
**Method #2**) Use deps file
	
	[wbxFileBundle]
		git=http://github.com/ma2thieu/wbxFileBundle.git
		target=bundles/wbx/FileBundle


**Method #3**) Use git submodules

    git submodule add git://github.com/ma2thieu/wbxFileBundle.git vendor/bundles/wbx/FileBundle


Register the wbxFileBundle namespaces (Not required for composer!)
------------------------------------------------------------------

    // app/autoload.php
    $loader->registerNamespaces(array(
        'wbx'  => __DIR__.'/../vendor/bundles',
        // your other namespaces
    ));


Add wbxFileBundle to your application kernel
--------------------------------------------

	// app/AppKernel.php
    public function registerBundles()
    {
        return array(
            // ...
            new wbx\FileBundle\wbxFileBundle(),
            // ...
        );
    }


Configuration
=============

	# app/config/routing.yml :
	wbxFileBundle_download:
		pattern:  /wbxfile/download/{class}/{id}
		defaults: { _controller: wbxFileBundle:File:download }


Usage example
=============

Entity
------

	# /src/my/Bundle/Entity/File.php

	namespace my\Bundle\Entity;
	use wbx\FileBundle\Entity\File as wbxFile;

	/**
	 * wbx\CoreBundle\Entity\File
	 *
	 * @Orm\Entity()
	 * @ORM\Table()
	 */
	class File extends wbxFile {

	}


	# /src/my/Bundle/Entity/Object.php
	/**
	 * @var \my\Bundle\Entity\File $file
	 *
	 * @ORM\ManyToOne(targetEntity="\my\Bundle\Entity\File", cascade={"all"})
	 * @ORM\JoinColumn(name="file_id", referencedColumnName="id")
	 */
	private $file;
	

Form
----

	$builder
		->add('file', new \wbx\FileBundle\Form\FileType(true|false))

* Pass `true` or `false` to `FileType` constructor to display or not the "empty" checkbox to remove the uploaded file


View
----

	{% include 'wbxFileBundle:File:embed.html.twig' with {
		'form'              : edit_form.image,
		'class'             : "myBundle:File",
		'imagine_filter'    : "my_thumb_filter"
	} %}

*   `form` is mandatory
*   `class` is mandatory (used for the download link)
*   `imagine_filter` is optional: if set the bundle [LiipImagineBundle](http://github.com/liip/LiipImagineBundle) is needed and the value of `imagine_filter` will be used as a the name of the Imagine filter to be called to create and display a thumbnail of the picture.
    If `imagine_filter` is not defined or == "" a link to the file will be displayed
