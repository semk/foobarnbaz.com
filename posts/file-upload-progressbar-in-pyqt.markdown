---
title: Simple file upload progressbar in PyQt
date: '31-12-2012'
time: '14:45'
tags: ['Python', 'PyQt', 'Qt', 'GUI', 'QProgressDialog', 'urllib2', 'StringIO']
layout: 'post.html'
---

I've been learning the [Qt](http://qt-project.org/) Framework for the last few months and righfully it took 
the honour of being my favourite GUI toolkit ;-). But still I don't like KDE. I have used 
[Tkinter](http://wiki.python.org/moin/TkInter) for quite some time when I was studying engineering. But Qt is 
much more powerful than Tkinter and comes with an enormous bundle of GUI widgets and elements. Qt documentation
is one of the best documentations available for a GUI framework. I must admit that I started liking Qt even more 
as soon as I started reading the documentation and trying out sample programs.

I encountered a challenge of displaying a progress dialog for files being uploaded to a central server. In this 
case the files were uploaded using sending a POST request to the server using the `urllib2` library. The data was 
multipart encoded. The problem here was, when you send the data to the server, how would you show the incremental 
progress of how much data got uploaded in the progress dialog view? The `urllib2` library doesn't provide any special 
methods to help in this situation. Neither does `Qt`. But we know the upper and lower limits of the progress bar: 0 to 
size of the file. But how would you tell Qt that how much data is uploaded at a given time?

Well, I found a tricky way to let Qt know about the file upload status using the `StringIO` module. The solution is to give 
`urllib2` a custom `StringIO` instance that will periodically alert `QProgressDialog` about the status of the upload. 
Checkout my approach to this problem.

    :::python
    class CancelledError(Exception):
        """Error denoting user interruption.
        """
        def __init__(self, msg):
            self.msg = msg
            Exception.__init__(self, msg)

        def __str__(self):
            return self.msg

        __repr__ = __str__


    class BufferReader(StringIO):
        """StringIO with a callback.
        """
        def __init__(self, buf='', 
                     callback=None, 
                     cb_args=(), 
                     cb_kwargs={}):
            self._callback = callback
            self._cb_args = cb_args
            self._cb_kwargs = cb_kwargs
            self._progress = 0
            StringIO.__init__(self, buf)

        def read(self, n=-1):
            """Read the chunk. Alert the callback.
            """
            chunk = StringIO.read(self, n)
            self._progress += int(len(chunk))
            self._cb_kwargs.update({'progress': self._progress})
            if self._callback:
                try:
                    self._callback(*self._cb_args, **self._cb_kwargs)
                except: # catches exception from the callback
                    raise CancelledError('The upload was cancelled.')

            return chunk

We can use the `BufferReader` instance as the input to the `urlllib2.Request`. It takes the actual data to be
sent as the input and registers a callback function which will be periodically called whenever a read occurs from
the `urllib2` module. Let's create a progress dialog box in Qt which shows the progress.
    
    :::python
    # create the progress dialog
    progressDialog = QProgressDialog('Uploading %s ...' % file_path,
                                     QString("Cancel"), 0, file_size)
    progressDialog.setWindowTitle('Upload status')
    # create a file stream that supports callback
    databuf = BufferReader(buf=body, callback=update_progress,
                           cb_args=(progressDialog, file_size))

    # upload the file using the databuf
    req = urllib2.Request(URL, databuf, headers)
    urlobj = urllib2.urlopen(req)
    result = urlobj.read()

And we can use the callback function `update_progress` to update the progressbar with additional `progress` keyword argument 
which reports the bytes uploaded/read by the `urllib2` library.

    :::python
    def update_progress(progressbar, size, progress=None):
        """Callback method for reporting upload status.
        """
        if progressbar.wasCanceled():
            raise # notify uploader and stop file upload

        progressbar.setLabelText('Uploading %d KB of %d KB'
                                 % (progress/1024, size/1024))
        progressbar.setValue(progress)
        QApplication.processEvents()

And this happens to be my last blog post of 2012. Happy to have survived the apocalypse, no zombies, earthquakes or Tsunamis. 
Wishing you all a prosperous and happy new year. Welcome 2K13.
