# Block unwanted visitors through nginx

Every website has it's own share of unwanted visitors. Crawlers, spam bots, DDoS-attacks... everything may happen.

Using this configuration, you can block unwanted visitors from your website. Most types of crawlers are being blocked.

## How it works

Insert the file `block.conf` into your nginx configuration directory and include it in your nginx using the following configuration:

	http {
        ...
        include block.conf;
        ...
 	}
 	
This will include the variables into your configuration.

### And now let's get to the blocking

After you've included the `block.conf` file into your nginx configuration, you can start blocking the bots from your server-blocks.

	server {
		...
		location / {
		    # A bad bot has been found, return a 403 error and don't show them in logs
		    if ($limit_bots = 1) {
			    return 403;
		    }

		    # Bad referer has been found. Throw 403 error to deny access for the user and don't log them
		    if ($bad_referer = 1) {
			    return 403;
		    }

		    # Only allow GET, HEAD and POST requests. If another request is thrown,
		    # throw a 444 error (No Reponse) to close the connection
		    if ($request_method !~ ^(GET|HEAD|POST)$) {
		        return 444;
		    }
		}
	}

## Thanks to

[mariusv](https://github.com/mariusv/nginx-badbot-blocker)