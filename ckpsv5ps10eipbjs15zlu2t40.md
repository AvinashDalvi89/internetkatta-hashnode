## How to create custom plugin in Mautic

Hello Developers,

Here I am going to explain about how to create a custom plugin in  [Mautic](https://www.mautic.org/) platform. Let me first introduce what Mautic is and why to use Mautic. 

**What is Mautic **? 

**Mautic** is an open source software platform which is used to manage marketing automation such as lead generation, email marketing, automation of calls etc. If you are looking for a contribution to Mautic then [here is link ](https://github.com/mautic/mautic).

<iframe width="560" height="315" src="https://www.youtube.com/embed/yKgaIoElsWU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


**What is a custom plugin and why is it required ?** ? 

If you are already familiar with open source software like WordPress, Magento, Drupal, Joomla then you might know about the custom plugin concept and how it helps. 

If you are new to this then, here is the answer for you. 

Custom plugin is none other than extension of your core functionality of software which enables you to add extra features than existing one without overriding core files or core logic. A simpler way is **Function Overriding** in programming. If you have a parent class then you extend it as a child class and you can override the function of the parent class by using the same name and parameters. Exactly similar custom plugin work. Custom plugin is none other than a child class of core functionality or feature provided in code framework to extend functionality. Like shown in below image👇🏻 its getting added extension to switchboard.
![socket.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1623447630165/VnG0zYcyD.png)

**How does it help ?**

- Keeping safe upgrade
- Give more Control over functionalities
- Plugins help you to extend **Mautic** however you need

Lets jump onto the actual topic about creation of custom plugins in **Mautic**. 

**Steps for creation of custom plugin**:

-  **Purpose of plugin** : Decide name and purpose of plugin so that you can use the same in plugin name and folder structure while creating in Mautic. If you are creating for third party integration then specify the same else if you're extending Mautic existing functionality like adding extra UI components or customising of UI elements etc. 

- Create a folder inside the `plugins` folder. Let's take an example: you are going to create a plugin named `ExampleBundle`. So create a folder with the same name folder. 

- Create `ExampleBundle.php` file inside `plugins->ExampleBundle`. Copy below code in file. 

```php
<?php 
namespace MauticPlugin\ExampleBundle;

use Mautic\PluginBundle\Bundle\PluginBundleBase;

class ExampleBundle extends PluginBundleBase
{
}
?>
```
- Create a  `Config` folder inside `ExampleBundle`. Then create a file `config.php` inside the folder `ExampleBundle->Config`. Copy below code and make changes accordingly.

```
<?php

// plugins/ExampleBundle/Config/config.php

return array(
    'name'        => 'ExampleBundle',
    'description' => 'Add here description of plugin',
    'author'      => 'Avinash Dalvi', // Change this to  plugin author 
    'version'     => '1.0.0', // Change this version to your appropriate version
    'routes'   => array( 
    
    )
);

 ```

- Folder structure will look like below. Other folders are as per customisation which you would like to do. 

![Folderstructure.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1623077183460/2-fFdCQDc.png)

- For the icon of the plugin keep the icon image in either .png or .jpeg format under `Assets/img` if the folder is not there then create an `Assets/img` folder and copy the image file there. 

- Run `php bin/console cache:clear` to clear Mautic cache. 
- Go Muatic website link -> Setting (Right hand side top setting icon ) -> Plugins 
- Click on "Install/Upgrade Plugins" then it will reflect the plugin icon along with name. 
- For overriding existing Controller/Event follow  [Symfony framework override guideline](https://symfony.com/doc/4.4/bundles/override.html) . 

![Screen Shot 2021-06-12 at 2.41.45 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1623445949679/WtrZEe3GK.png)

Now we are done with the creation of a custom plugin. 

**What are use cases where can use custom plugin** :

- Third party lead management system integration like Dialer system, Landing page form etc. 
- Changing default unique parameter while importing or creating lead in Mautic 
- Customisation of process flow like addition of extra flow in automated flow. 

I hope this helped you in understanding how to create a custom plugin. If you have any Queries or Suggestions, feel free to reach out to me in the Comments Section below or my twitter handle [@AvinashDalvi_](https://twitter.com/AvinashDalvi_).

I have created one [Mautic Plugin Creator](https://github.com/AvinashDalvi89/mautic-plugin-creator).  This is just bundle creation to avoid manual effort which is available on :
- https://github.com/AvinashDalvi89/mautic-plugin-creator
- https://packagist.org/packages/aviboy2006/mautic-plugin-creator

If you like, don't forget to give stars. 

**References** : 

- https://www.mautic.org/what-is-mautic
- https://www.mautic.org/blog/developer/how-to-create-a-mautic-plugin-tutorial-series-introduction
- https://symfony.com/doc/5.2/bundles/override.html
- https://github.com/mautic/plugin-helloworld