###
FAQ
###

Why is this library slow?
===========================

The ``send`` and ``validate_utf8`` methods are very slow in pure Python.
If you want to get better performance, please install both numpy and wsaccel.
Note that wsaccel can sometimes cause other issues.

How to solve the "connection is already closed" error?
===========================================================

The WebSocketConnectionClosedException, which returns the message "Connection
is already closed.", occurs when a WebSocket function such as ``send()`` or
``recv()`` is called but the WebSocket connection is already closed. One way
to handle exceptions in Python is by using
`a try/except <https://docs.python.org/3/tutorial/errors.html#handling-exceptions>`_
statement, which allows you to control what your program does if the WebSocket
connection is closed when you try to use it. In order to properly carry out
further functions with your WebSocket connection after the connection has
closed, you will need to reconnect the WebSocket, using ``connect()`` or
``create_connection()`` (from the _core.py file). The WebSocketApp ``run_forever()``
function automatically tries to reconnect when the connection is lost.

What's going on with the naming of this library?
==================================================

To install this library, you use ``pip3 install websocket-client``, while ``import
websocket`` imports this library, and PyPi lists the package as
``websocket_client``. Why is it so confusing? To see the original issue about
the choice of ``import websocket``, see
`issue #60 <https://github.com/websocket-client/websocket-client/issues/60>`_
and to read about websocket-client vs. websocket_client, see
`issue #147 <https://github.com/websocket-client/websocket-client/issues/147>`_

How to disable ssl cert verification?
=======================================

Set the sslopt to ``{"cert_reqs": ssl.CERT_NONE}``. The same sslopt argument is
provided for all examples seen below.

**WebSocketApp example**

::

  ws = websocket.WebSocketApp("wss://echo.websocket.org")
  ws.run_forever(sslopt={"cert_reqs": ssl.CERT_NONE})


**create_connection example**

::

  ws = websocket.create_connection("wss://echo.websocket.org",
    sslopt={"cert_reqs": ssl.CERT_NONE})

**WebSocket example**

::

  ws = websocket.WebSocket(sslopt={"cert_reqs": ssl.CERT_NONE})
  ws.connect("wss://echo.websocket.org")


How to disable hostname verification?
=======================================

Please set sslopt to ``{"check_hostname": False}``. (since v0.18.0)

**WebSocketApp example**

::

  ws = websocket.WebSocketApp("wss://echo.websocket.org")
  ws.run_forever(sslopt={"check_hostname": False})

**create_connection example**

::

  ws = websocket.create_connection("wss://echo.websocket.org",
    sslopt={"check_hostname": False})

**WebSocket example**

::

  ws = websocket.WebSocket(sslopt={"check_hostname": False})
  ws.connect("wss://echo.websocket.org")

How to enable `SNI <http://en.wikipedia.org/wiki/Server_Name_Indication>`_?
============================================================================

SNI support is available for Python 2.7.9+ and 3.2+.
It will be enabled automatically whenever possible.

Why don't I receive all the server's message(s)?
===================================================

Depending on how long your connection exists, it can help to ping the server to
keep the connection alive. See
`issue #200 <https://github.com/websocket-client/websocket-client/issues/200>`_
for possible solutions.

Using Subprotocols
====================

The WebSocket RFC
`outlines the usage of subprotocols <https://tools.ietf.org/html/rfc6455#section-1.9>`_.
The subprotocol can be specified as in the example below:

>>> ws = websocket.create_connection("ws://example.com/websocket",
  subprotocols=["binary", "base64"])
