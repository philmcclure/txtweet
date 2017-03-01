# txtweet

`txtweet` is a bash script client for [twtxt][1], a decentralised, minimalist microblogging service for hackers.

# installation

Put `txtweet` somewhere in your `$PATH` and execute it.  It will generate the `$CONF_DIR` and populate it with everything you need to get started. Once it finishes, open `$HOME/.config/txtweet/config` in your favorite editor.

* Change your `nick` from "txtweet user" to whatever you prefer.
* By default, `twtxt.txt` is kept in the `$CONF_DIR`, you can move it somewhere else and change the `twtxtfile` variable to point to it. Alternatively, you can leave it in the `$CONF_DIR` and copy it elsewhere with a [posthook][2]
* By default, we truncate anything over 140 characters. You can change this by modifying the `character_limit` variable.
* If you call `twtxtfile` with no arguments, it will autoscroll your timeline.  By default, it will post the latest 20 tweets from the people you are following. If you want more, you can change the `timeline_count` variable.
  * If your timeline scrolls past too fast to read, you can pipe it through your favorite pager of choice (less, more, most, etc.).

# usage

    txtweet -u NICK -f FEED_SRC //follow a new user
    txtweet -l                  //list everyone you're following
    txtweet -t "TWEET BODY"     //post a new tweet
    txtweet -r NICK             //unfollow a user
    txtweet -v NICK             //show feed of user you're following

This is all available from the script if you call it with the help flag, `txtweet -h`.

# <a name="posthook">posthook</a>
In the `$CONF_DIR` there is a file called `posthook`.  It's generated by default when the script runs for the first time. 

The idea is, when you generate your `twtxt.txt` file, you might want to copy it somewhere else. You might not.  You might have `twtxt.txt` symlinked to your webroot, and it can safely stay in the `$CONF_DIR`, *but* you might want to `scp` it to your webserver. 

The script checks to see if the `posthook` file exists, and sources it into the main script if it does.  If you don't need it, you can safely delete the `posthook` file and it won't affect anything.  The default that is included only `echo`'s an empty line to the screen, so if you leave it, *as is*, it won't affect the script in any way.

In any event, here's an example of a `posthook` that will `scp` your `twtxt.txt` file to a remote server.

    posthook() {
        # Anything you need to happen after you tweet needs to go
        # into this function. This is just a placeholder."

        echo "Uploading twtxt.txt to the server..."
        scp $twtxtfile user@webserver:/var/www/
        echo "Upload finished!"
    }

[1]: https://twtxt.readthedocs.io/en/latest/
[2]: #posthook