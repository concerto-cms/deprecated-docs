#List of console commands
## CoreBundle
### concerto:create:language
Adds a new language to your content. The command will ask for following information:
- Iso-code: The ISO language/country code, ex. en-UK or fr-FR. 
- Full name: the full name of the language. It's recommended to describe the language in the correct language: English, Français, Deutsch, ...
- Prefix: The prefix is used for the generated url's, ex. /<strong>en</strong>/about.

### concerto:create:menu
Adds a new menu. Each website generally has at least a "main menu", but you can have additional menus such as the footer. The command will ask for following information:
- Name (Id): a url-friendly id for the new menu (ex. main-menu)
- Label: A label used in the CMS to describe the menu (ex. Main menu)

### concerto:create:menuitem
Create a new menu item. The command will ask you for following information:
- Target menu name: Name (id) of the target menu (ex. main-menu)
- Locale: the prefix of the language you wish to use for the added menu item
- Path: The path to the parent menu (ex. about/team)
- Name (id): Url friendly name of the new menu item
- Label: Label of the menu item
- Url: Target url for the link. This can be an internal url (/en/about) or an external url (http://www.google.be)

### concerto:fixtures:load
Add some demo data, such as a menu, a few menu items, several languages and some pages. This command is useful for demo purposes but is also used for testing.
