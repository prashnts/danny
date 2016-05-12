# (Danny) Phantom JS transparent proxy

Several pages require javascript to properly render urls. Several pages also require interactions in form of `mouse clicks`, `scroll down` etc. These all make the regular task of scraping very difficult.

A possible solution to this is by using `Web Drivers` which essentially allows us to control a web browser(ish) from within code. This allows us to perform mouse clicks etc. automatically hence solving the problem. In python, the first name that comes into mind is `Selenium` which can work with Chrome and Firefox.

However, the general `Chrome` and `Firefox` webdrivers are not `headless`, that is, they have a UI layer which makes it unsuitable to be run on a Server which does not necessarily has a GUI layer (such as GTK, Gnome, etc.). There are some "solutions" which can hack aroung this limitation to run Selenium headless, however with such solutions come great limitation of reliability.

Phantom JS is a headless webkit driver that works with JavaScript. It has a huge community support and reliable testing constructs.

## Danny

The basic idea behind Danny is to offload the task of interacting with DOM using expressive JSON syntax and return the **resulting DOM** to the client.

The following is a possible case demonstrating how Danny is useful:

- A scraper needs to access http://foo.com/bar which requires JavaScript.
- Rather than requesting the url directly, the scraper uses this proxy:
```python
>>> params = {
>>>   'url': 'http://foo.com/bar',
>>>   'actions': [
>>>     'render',                   # Instruct to render the page
>>>     {'click': 'button#next'} ,  # Click button with ID next
>>>   ],
>>>   'ttl': 120                    # Keep the page open for 120 seconds
>>> }
>>> response = requests.get('http://danny.local', params=params)
>>> rendered_dom = response.raw
```
- The server API provided by Danny is just a sequence of actions that are performed on the DOM. Once all the actions are finished, the **resulting DOM** is returned as a response.
- The scraper can then extract the data as if it were a regular HTML page.

