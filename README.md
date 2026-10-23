# PPT Remote

PPT Remote lets you control a PowerPoint slideshow from your phone using
a browser on the same local network. It runs a small web server on your
computer and displays a temporary six-digit PIN for connecting.

When you explicitly enable **Allow phone to see and control this PC**,
the phone can also view the computer's primary screen and use touch
controls for mouse, keyboard, and typing actions.

## Features

-   Next and previous slide controls
-   Start slideshow, start from current slide, and end slideshow
-   Jump to the first or last slide
-   Black/white presentation screen and pointer shortcut
-   Optional screen sharing and touch-based PC control
-   PIN-protected requests with temporary lockout after repeated
    incorrect PIN attempts
-   Works over a local network; an internet connection is not required

## Prerequisites

-   A computer running Windows (the included executable build script
    targets Windows).
-   Python 3.10 or newer recommended for running from source.
-   A phone and computer connected to the same Wi-Fi/LAN, or to a phone
    hotspot shared with the computer.
-   PowerPoint or another presentation application open on the computer.
-   Permission to allow Python/PPT Remote through the computer's
    firewall on your trusted private network.

The phone does not need to install a separate app: use its camera to
scan the QR code (if shown) or open the address in a browser.

## Project files

-   `server.py` --- local HTTP server, PIN authentication, slide
    actions, and optional screen stream.
-   `remote.html` --- phone-friendly remote-control web interface.
-   `requirements.txt` --- Python dependencies.
-   `build_exe.bat` --- builds a standalone Windows executable.
-   `make_icon.py` --- generates the application icons.
-   `PPTRemote.spec` --- PyInstaller build configuration.

## Setup and run from source

1.  Extract the project ZIP to a folder.

2.  Install Python 3.10+ if it is not already installed. During Windows
    installation, enable **Add Python to PATH**.

3.  Open Command Prompt or PowerShell and change to the extracted
    project directory (the directory containing `server.py`).

4.  Install dependencies:

    ``` bash
    python -m pip install --upgrade pip
    python -m pip install -r requirements.txt
    ```

5.  Start PPT Remote:

    ``` bash
    python server.py
    ```

6.  A desktop window should show the local address and six-digit PIN. It
    may also display a QR code.

7.  Keep this window open while using the remote.

If `python` is not recognized on Windows, try `py` in place of `python`.

## Connect your phone

1.  Make sure the phone and computer are on the same Wi-Fi/LAN. A guest
    Wi-Fi network may block devices from communicating with each other.
2.  On the computer, read the address displayed by PPT Remote, for
    example `http://192.168.1.25:8765/`. Use the actual address shown on
    your computer.
3.  On your phone, scan the displayed QR code or enter that address in
    the phone's browser.
4.  Enter the six-digit PIN shown in the desktop window, then tap
    **Connect**.
5.  Open your presentation on the computer and use the phone controls to
    navigate the slides.

The address and PIN can change each time the program starts. Use the
current values shown by the desktop app.

## Using the controls

The remote's slide controls include the following actions:

  -----------------------------------------------------------------------
  Action                              Result
  ----------------------------------- -----------------------------------
  Next / Previous                     Move forward or backward one slide

  Start                               Start the slideshow from the
                                      beginning

  Start current                       Start the slideshow from the
                                      current slide

  End                                 Exit the slideshow (Escape)

  First / Last                        Go to the first or last slide

  Black / White                       Toggle the presentation's black or
                                      white screen shortcut

  Pointer                             Send the configured pointer
                                      shortcut

  Screen sharing & touch control      When enabled on the computer, show
                                      the primary display and allow
                                      remote mouse/keyboard actions
  -----------------------------------------------------------------------

The slide controls send keyboard shortcuts to the computer, so the
presentation application should be open and ready. Some shortcuts may
behave differently in applications other than Microsoft PowerPoint.

### Enable screen sharing and full control

Screen sharing and touch-based PC control are **off by default**.

1.  On the computer, select **Allow phone to see and control this PC**
    in the PPT Remote window.
2.  Use the phone's screen/pointer controls as needed.
3.  Turn the option off when finished.

When enabled, a connected user with the PIN can see the primary display
and interact with the computer, including mouse and keyboard input. Only
enable this on a network you trust, and disable it when you no longer
need it.

## Build a standalone Windows executable

You can build `PPTRemote.exe` so Python does not need to be installed on
the target computer.

1.  Install Python on the build computer and extract the project.

2.  Open Command Prompt in the project folder.

3.  Run:

    ``` bat
    build_exe.bat
    ```

4.  When the build completes, find the executable at:

    ``` text
    dist\PPTRemote.exe
    ```

The batch file installs the requirements and PyInstaller, generates
`icon.ico` and `icon.png` if the ICO is missing, then packages the
server and web interface. If Windows Defender Firewall asks for
permission, allow access only on a trusted private network.

## Network and security notes

-   PPT Remote listens on port **8765** on the computer's network
    interfaces.
-   The PIN is generated when the program starts; share it only with
    people you trust.
-   Five incorrect PIN attempts from one device trigger a five-minute
    lockout for that device. Repeated failures across devices can also
    trigger a global temporary lockout.
-   The PIN is a local-network access check, not encrypted transport. Do
    not expose port 8765 to the public internet or configure router port
    forwarding for it.
-   Anyone with access to the PIN can send slide-control requests. If
    screen sharing/full control is enabled, that person can also
    interact with the PC.
-   Turn off screen sharing/full control when it is not required, and
    close the program when finished.

## Troubleshooting

### The phone cannot open the address

-   Check that both devices are on the same Wi-Fi/LAN or hotspot.
-   Use the exact address shown in the desktop window, including
    `:8765`.
-   Avoid guest Wi-Fi networks or access points with client isolation
    enabled.
-   Allow Python or `PPTRemote.exe` through Windows Firewall on a
    trusted private network.
-   If several addresses are shown, try another address listed by the
    app.

### The PIN is rejected or connection is temporarily blocked

-   Enter the current six-digit PIN shown in the computer window.
-   If the app has been restarted, use its new PIN.
-   After five incorrect attempts from one device, wait five minutes
    before trying again.

### Slide buttons do not work

-   Make sure the presentation application is open on the computer.
-   Click the presentation/slideshow window on the computer once, then
    try again.
-   Check whether the keyboard shortcuts listed above are supported by
    the presentation application.

### Screen sharing does not start

-   Enable **Allow phone to see and control this PC** in the desktop
    window.
-   Keep the desktop app running.
-   Check firewall/network settings and reload the remote page.
-   Screen sharing captures the primary display. The server allows up to
    two simultaneous screen streams.

### Missing dependencies or Python errors

Run the dependency installation again:

``` bash
python -m pip install -r requirements.txt
```

For executable-build problems, try:

``` bash
python -m pip install --upgrade pyinstaller
```

## Stop the server

Close the PPT Remote desktop window or stop the Python process from the
terminal where it was started. The phone will no longer be able to
control the computer after the server stops.

## License

MIT License
