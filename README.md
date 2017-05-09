# osx-network-configurer
If you are working 
- with a Apple MacBook,
- often switching locations and 
- some of your locations might have some "special requirements" regarding internet access (via proxy or vpn)

this set of scripts might be handy. 

Basically there are two main components, which are managed via launchctl and could be installed and uninstalled via the setup script.

## location-changer
Whenever the connected wireless network (SSID) changed, the scripts tries to switch to an equally named location. If none is found, the default one ("Automatic") is choosen. This mapping could be customized by having a config file in _~/.locations/location.conf_, containing key/value pairs (separated by "=") of wireless network SSID = location name, so multiple different SSIDs could be mapped to the same location for example.

## location-changed
Whenever the location changed, the script runs all user-specific scripts, which are potentially located in _~/.locations/[LOCATION_NAME]_, with the location name as first parameter. If there is a need to run scripts regardless of the target location, you can place these script in _~/.locations/general/_. These are picked up and run, every time the location changed.

For convenience I shipped to handy scripts, which are installed by default:
### general/display-notification
Will simply display a osx notification with the current location.

### general/export-proxy-settings 
Reads the current location's proxy configuration from Mac OS and generated a file _~/.bash_proxy_settings_, containing bash environment variable exports (http_proxy, https_proxy, no_proxy) for these proxy information, which could simply be included in your _~/.bash_profile_, like
<pre><code>
if [ -f .bash_proxy_settings ]; then
  source .bash_proxy_settings
fi
</code></pre>
so your bash tools (at least the ones honoring these variables) are able to connect.
