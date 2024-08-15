---
title: "Creating Bookmarks in Badwolf Browser: A Step-by-Step Guide"
date: 2023-03-09
---

# Step 1: Creating the Bookmarks File
To begin, you need to create a file called `${XDG_DATA_HOME:-$HOME/.local/share}/badwolf/bookmarks.xbel` in a specific format. The `${XDG_DATA_HOME:-$HOME/.local/share}` part represents an environment variable that points to the user's data directory. If it's not set, it defaults to `$HOME/.local/share`. Ensure that the directory structure exists before proceeding.

# Step 2: Formatting the Bookmarks File
Open a text editor and paste the following code into the file:

```
<?xml version="1.0"?>
<!DOCTYPE xbel>
<xbel version="1.0">
    <bookmark href="https://duckduckgo.com">
        <title>Duckduckgo search</title>
    </bookmark>
    <bookmark href="https://search.sapti.me/">
        <title>Custom Search Engine</title>
    </bookmark>
</xbel>
```

In this example, we have included two bookmarks: one for Duckduckgo and another for a custom search engine. Feel free to modify or add more bookmarks as per your preferences. Make sure to enclose each bookmark within the `<bookmark></bookmark>` tags, providing the href attribute for the URL and the `<title></title>` tag for the bookmark's title.

# Step 3: Saving the Bookmarks File
After formatting the bookmarks in the desired manner, save the file with the name `bookmarks.xbel`. Ensure that the file extension is `.xbel` to adhere to Badwolf's bookmark file format.

# Step 4: Exploring Bookmarks
As you start typing URL in the address bar, Badwolf will suggest matching bookmarks from your `bookmarks.xbel` file. Simply select the desired bookmark from the suggestion list, and Badwolf will immediately navigate to the corresponding URL.
