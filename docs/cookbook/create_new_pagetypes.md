# Create new page types
By default, the CMS only allows the user to create "basic pages" which only have a title and a content editor. There are a lot of use cases where you would want to change or expand the form used to edit a type of page. Some examples:
- Instead of fiddling with a table using the HTML editor, A restaurant website could have an "opening hours" page where the user simply fills in the table (possibly with sliders)
- A page containing a "carousel" or some kind of photo gallery.
- A product page with PDF downloads, such as brochure, technical specs, manual, ...

Concerto allows you to define new page types, which gives you much freedom:
- Each page type can have its own form view to edit the page.
- Add some constraints so certain pages can only have certain page types as children
- The CMF routingBundle allows you to point each page type to its own controller. This allows you to use your own Twig template to render a page for the given page type

Let's start with a simple page without a custom form. First, we need to create a new PHP class that implements ContentInterface, or extend the basic Page class:

```
use Doctrine\ODM\PHPCR\Mapping\Annotations as PHPCR;

/**
 * @PHPCR\Document(referenceable=true)
 */
class Product extends \ConcertoCms\CoreBundle\Document\Page
{

    public function getClassname()
    {
        // We must specify a unique name for the new page class
        return "YourBundle:Product";
    }
}
```
As you can see, not much interesting is happening. We must override getClassname to give our class its own unique name. PHPCR uses annotations to define database parameters, very similar to the Doctrine ORM library.

You must also create a service that implements the ```PageManagerInterface```. The service has a few purposes:
- Define ```PageType``` objects that will identify your class as a page type
- Provide event listeners that are called whenever a new page is created or an existing page is being updated

```
use YourBundle\Document\Product;
use ConcertoCms\CoreBundle\Extension\PageManagerInterface;
use ConcertoCms\CoreBundle\Extension\PageType;
use ConcertoCms\CoreBundle\Event;

class PageManager implements PageManagerInterface {
    /**
     * @return \ConcertoCms\CoreBundle\Extension\PageType[]
     */
    public function getPageTypes()
    {
        $product = new PageType(
            "YourBundle:Product",
            "Product page",
            "View.PageContent_Page"
        );
        return array($product);
    }

    /**
     * @param Event\PagePopulateEvent $event
     * @return void
     */
    public function onPopulate(Event\PagePopulateEvent $event)
    {
        // Do nothing since the ConcertoCms PageManager deals with all default fields
    }

    /**
     * @param Event\PagePopulateEvent $event
     * @return void
     */
    public function onCreate(Event\PageCreateEvent $event)
    {
        if ($event->getType() == "YourBundle:Product") {
            $event->setDocument(new Product());
        }
    }
}
```
The return value of getPageTypes is an array, so you can define several pagetypes in a single PageManager service.

Configure your service in services.yml:
```
services:
    yourbundle.pagemanager:
        class: YourBundle\Service\PageManager
        tags:
            - { name: concerto.pagemanager }

```
Note the tag "concerto.pagemanager". This will make sure the PageTypes are properly configured and the event listeners are added to the event dispatcher.

Finally, you might want to point your new page type to its own controller:action whenever its being visited:
```
cmf_routing:
    dynamic:
        generic_controller: \YourBundle\Controller\DefaultController::pageAction
        controllers_by_class:
            YourBundle\Document\Product: \YourBundle\Controller\DefaultController::productAction

```

The user can now create new "Product" pages in the CMS. Whenever the page is called in the website, it will be routed to the productAction instead of the geneic controller.
