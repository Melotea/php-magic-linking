# PHP Magic Linking Template

**NEVER WORRY ABOUT PUTTING ../ BEFORE LINKS EVER AGAIN!**

You often store each page in a separate folder to allow more readable urls. Small sites can get away with having `index.php`, `about.php`, `gallery1.php`, `gallery2.php` etc in the same folder, but it's not very organized or scalable. Instead of `mySite.com/about_us_page.php`, you want `mySite.com/about/`. You putting an page  `index.php` in the a folder `about/`. (Of course `mySite.com/about` would be identical to `mySite.com/about/index.php` in this case)

You can have multiple layers of sub-directories: a main Gallery Page `mySite.com/gallery` that lists the individual sub-galleries like ``mySite.com/gallery/indoor` or `mySite.com/gallery/outdoor`

But you run into trouble linking between pages. If all pages are in the same folder, you can use **1 copy of global navigation** called `nav.php` & use a **`php include`** to keep the same navigation menu on all pages. But nested sub-directories cause problems.

<s>A cheater "solution"  is to have a copy of `nav.php` for each level of directories</s><br>
DON'T TRY THIS! Trust me, it's a pain to maintain 3 copies of the same file

###The Situation (Problematic Linking)###
 - Going **from the** ***Homepage*** (`index.php` in the root folder of `mySite.com`) **to the** ***About Page***  would be **`href="about/"`** or `href="about/index.php"`
 - Linking **2-directories** down would be **`href=gallery/indoor/`**
 - Linking **from the** ***homepage*** always goes **down into** sub-directories
 - &nbsp;
 - **From the** ***About Page*** back up **to the** ***homepage*** would be: **`href="../"`** or `href="../index.php"`
 - Linking **to a** ***Gallery Page*** would be **`href="../gallery/indoor`**
 - &nbsp;
 - And **from the** ***Indoor galley*** all the way **to the** ***About Page*** would be `href="../../about/"`
 - &nbsp;
 - &nbsp;
 - **3 different links to the same galley page is no good!**
 <br>All based on where the folders are in relationship to each other

##The Solution##
- **Act as if all links start in the root folder**
- This code analyzes the URL to find how many folders below the **root directory** the current page is
- **Add `<?php echo $upFolderPlaceHolder ?>` before any link**
<br>This manual link **from** ***About*** **to** ***Home*** `<a href="index.php">Home</a>`
<br>**Becomes `<a href="<?php echo $upFolderPlaceHolder ?>index.php">Home</a>` & will work from any level directory**
- Code in `top.php` magically prints the correct number or `../` in your link, making wherever the file is located

#[Download The Latest Release (Project Zip)](https://github.com/SleekPanther/php-magic-linking/releases/latest)#

##Major Features##
2. **Link to any page without worring about navigating up directories <br>**
(*also applies to css, images & any files on your site*)
1. Consistent nav on all pages (using the same `php include`)
3. Automatic breadcrumb links
4. Automatic meta descriptions stored in easily-editable text file
3. `<title>` Tags that match the current page
4. Print the **page name** in `<h1>` automatically (no hard-coding)
5. Identify the current page & highlight the current link in nav (so the user knows where they are in the site)
6. Print unique ID's for each page in `<body>` tags <br>
can use css to apply styles to **target on only 1 page**

##Code Details###

###Your Folder Setup###
- Every viewable should be called `index.php`
- But MUST HAVE UNIQUE PARENT FOLDER<br>
(Since they're all called `index.php`, **the *containing folder* is very important to identify the current page**)
- **Folder names should be lowercase NO SPACES**
- **Folder names BECOME the** ***Page Title***
- `-` (hyphen characters) will be replaced with `/` (forward-slashes)
- `_` (underscores) will be replaces with **spaces**
- The **fitst letter** of each word will be ***C*****apitalized**

**Example:** The folder "our_prices-services" becomes "Our Prices / Services"

###This Project's Default Folder Setup (what files go where)###
- The **main homepage** goes in the root directory of your entire project
- `about/index.php` gets its own folder since it's on the same level of the nav as **Home**
- `portfolio/index.php` and `tests/index.php` are also on the same level of the nav as **Home** <br>
*They're mostly placeholder pages used for breadcrumb trails*
- `portfolio/portfolio_1/index.php` and `tests/test_1/index.php` are **1-level dropdown** pages
- `portfolio/examples/example_1/index.php` is a **2nd-level dropdown** page
- `images/` contains ALL images<br>
There are many ways to organize your images, so modfy as you please
- `non-pages/` contains anything that ISN'T a complete page <br>
`php-include/` has partial components that are assembled to create complete pages
`css/` has the stylesheets
`descriptions/` has a tect file for Meta tag page descriptions

####THINGS YOU MUST EDIT!####
- Rename `php-magic-linking` to the name of your site **(or leave it alone if you don't care)**
- Edit line 12 in `top.php` to match **YOUR ROOT DIRECTORY** <br>
`$ROOT_DIRECTORY = "php-magic-linking";` (this should match, or be changed)
- Every page must manually link to `top.php` BEFORE ANY OTHER PHP CODE! <br>
**HomePage link:** `<?php include("non-pages/php-include/top.php"); ?>` <br>
**1 folder Level below homepage** (from `/about/index.php`, `/portfolio/index.php` etc.):  **`<?php include("../non-pages/php-include/top.php"); ?>`** <br>
**2 Levels below** (from `/portfolio/examples/index.php`):  **`<?php include("../../non-pages/php-include/top.php"); ?>`** <br>
**3 Level below** (from `/portfolio/examples/example_1/index.php`):  **`<?php include("../../../non-pages/php-include/top.php"); ?>`**
- &nbsp;
- Ctrl+F/Find/Search **"36714768356"** in `top.php` (section to list all YOUR pages)

1. Put all top-level pages (INCLUDING $ROOT_DIRECTORY for the **Homepage**) in `$pageArrayTop` array <br>
`$pageArrayTop = array($ROOT_DIRECTORY, 'portfolio', 'tests', 'about');` //this is the default code <br>
Replace with **EXACT FOLDER NAMES (case sensitive)** of YOUR pages
2. Put ALL 1st-level-dropdown pages in `$pageArrayDropDown1` array <br>
`$pageArrayDropDown1 = array ('portfolio_1', 'portfolio_2', 'examples', 'test_1', 'test_2');` //default code
3. Put ALL 2nd-level-dropdown pages in `$pageArrayDropDown1` array <br>
`$pageArrayDropDown2 = array ('example_1');` //default code

- ORDER DOESN'T MATTER, but don't leave out any pages!
qwerty
####Optional Things To Edit####
- STUFF 2 EDIT (favicon, logo, meta author, descriptions, tagline, )
- Breadcrumbs are optional. **To remove:** Simply delete the `<section class="breadcrumbs>` tag in `header.php` <br>
*Leave the code in `top.php` alone just in case you want them later
- a

###Page Structure (PHP Includes)###
Viewable pages are as follows

1. `top/php` begins the HTML file. It has everything in the `<head>` section & important URL magic happens here <br>

2. aaa

###Linking###
- Now subsequent PHP Includes can use `$upFolderPlaceHolder` to take care of how many times to prepend `../` to the link. <br>
`<?php include($upFolderPlaceholder . "non-pages/php-include/footer.php"); ?>`

####Linking Usage Examples####
- **Use `<a href="<?php echo $upFolderPlaceHolder ?>index.php">Home</a>` to link to "Homepage"** instead of `<a href="index.php">Home</a>`
- 9099

###Breadcrumb Links###



###Meta Tag Page Descriptions###



###Highlight Current Page in Nav###


###Easy `<title>` Tags###


###Print Automatic Page Name in `<h>`###


###Uniqe Page ID's in `<body>` tag###


##Extra Notes##
- This is a readme to demo **usage*, detailed comments in actual code
- Search "454521" using `Ctrl+F` or `Cmd+F` to skip to important sections in `top.php`
- The nav menu is adapted from [Pure CSS Mobile-compatible Responsive Dropdown Menu](http://www.cssscript.com/pure-css-mobile-compatible-responsive-dropdown-menu/) <br>
or check out [My adapted version on GitHub](https://github.com/SleekPanther/css-dropdown)
- `non-pages/css/menu.less` can be ignored if you aren't familiar with `.less` files. Basically it's a fancy version of CSS which is compiled to normal CSS yieldind the normal `non-pages/css/menu.css` file <br>
or check out [Less.js](http://lesscss.org/) to learn more

##Future Feature Goals##
- page structurte (top nav header content footer) EXPLAIN HEADER REARRANGEMENT (start of `<main>`)
