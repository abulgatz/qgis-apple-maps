## Instructions

### Windows

- Download nginx's **nginx/Windows** version from [here](https://nginx.org/en/download.html) and extract it.
- Download this repository.
- Move `start.bat`, `stop.bat` and `restart.bat` from this repository into the root of the extracted nginx portable directory.
- Open the provided `nginx.conf` file from this repository.
- Copy all of the lines from the file. Close the file.
- Open the `conf\nginx.conf` file from the extracted nginx portable directory.
- Add the copied lines right before the closing curly bracket `}` of the `http` block. This should be near the end of the file. Leave the file open.
- Go to Duck Duck Go Maps: <https://duckduckgo.com/?t=h_&q=Washington%2C+DC&iaxm=maps>.
- Open the browser inspector.
- Open the network tab.
- Clear the network tab.

#### Map View

- Enter filter: `cdn.apple-mapkit.com/ti/tile`
- Refresh the page.
- Copy any of the returned map tile URLs in the network tab.
- Extract the just the **values** of `v=` and `accessKey=` from the URL.
    - The `v=` value should just be a number. Eg: `12345`
    - the `accessKey=` value will start with a number and end in `%3D`. Eg: `1234567890_1234567890123456789_%2F_a1BCDefGHI2jklMnOP34Q%5Rs6Tu%7VwxYZabcdeFghIjkl%3D`
- Go back to the open `conf\nginx.conf` file from the extracted nginx portable directory.
- Under the `listen 8080` section, set the values of `$v` and `$access_key` (between the double quotes `""`) to the values you extracted above and save the file.
- Run `start.bat` from the nginx portable directory.
- Check `logs\error.log` to make sure that your config file is valid and nginx started without any errors.

#### Satellite View

- Enter filter: `sat-cdn.apple-mapkit.com/tile`
- Refresh the page.
- Switch to **Satellite** view.
- Copy any of the returned map tile URLs in the network tab.
- Extract the just the **values** of `v=` and `accessKey=` from the URL.
    - The `v=` value should just be a number. Eg: `12345`
    - the `accessKey=` value will start with a number and end in `%3D`. Eg: `1234567890_1234567890123456789_%2F_a1BCDefGHI2jklMnOP34Q%5Rs6Tu%7VwxYZabcdeFghIjkl%3D`
- Go back to the open `conf\nginx.conf` file from the extracted nginx portable directory.
- Under the `listen 8081` section, set the values of `$v` and `$access_key` (between the double quotes `""`) to the values you extracted above and save the file.
- Run `start.bat` from the nginx portable directory.
- Check `logs\error.log` to make sure that your config file is valid and nginx started without any errors.

#### QGIS

- Open QGIS.
- Go to **Layer** > **Data Source Manager** (or <kbd>CTRL</kbd> + <kbd>L</kbd>).
- Select **Browser** on the left sidebar.
- Expand **XYZ Tiles**.
- If you have any old versions of the Apple Maps connector, right click and delete them.
- Right click **XYZ Tiles** and select **Load Connections...**.
- Browse to **Apple_Proxy.xml** from this repository.
- Click **Select All** and then **Import**.
- Double click **Apple Maps Map Proxy** and/or **Apple Maps Satellite Proxy** under **XYZ Tiles** to add a layer to your project.
- When you are done run `stop.bat` from the nginx portable directory.

## Updating v and accessKey

The values of `v` and `accessKey` will eventually expire and you will start getting 403 and/or 404 errors in `logs` > `access.log`. QGIS should also pop up an access denied warning bar.

To fix this, follow the steps above under **Map View** and/or **Satellite View** depending on what you're using.

Then, you can restart nginx with `restart.bat` if it was already running, or start it with `start.bat` if it wasn't.