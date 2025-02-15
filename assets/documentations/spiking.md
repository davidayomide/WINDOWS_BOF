# Spiking

## What is Spiking?

Spike is protocol stress tests. Spiking is like fuzzing. The difference is that spiking send only garbage data with the aim of bringing down the security features of the system, while fuzzing is intended to get some useful information out of the data it sends.

Now we will use spiking technique to check if ``brainpan.exe`` buffer can be overflown. We will use ``spiking script`` in combination with a Linux tool called ``generic_send_tcp`` on Linux side. On Windows side, we will use [Immunity Debugger](https://www.immunityinc.com/products/debugger/ "Immunity Debugger website") to understand what is happening in the memory.

I write the following ``spinking script`` called [brainpan.spk](https://github.com/cris-m/buffer_overflow_exploit_development/blob/main/assets/resources/brainpan.spk) to send garbage data to ``brainpan.exe`` buffer.

```spk
s_readline();
s_string("shitstorm ");
s_string_variable("0");
```

On windows side I attached ``brainpan.exe`` to ``Immunity Debugger`` and start the process.

![alt Immunity debugger](../images/brainpan8.png  "Attached brainpan process to immunity debuger")

On Linux side, I execute the ``spiking script`` in ``generic_send_tcp``. Here is the output I got.

![alt Send garbage data](../images/brainpan9.png  "Execute spiking script")

After executing the script, I realized that at some point it could not connect to the target. I went in windows and check what was going on in ``Immunity Debugger``. I saw the ``Access violation notification`` in ``Immunity Debugger``.

![alt Immunity debugger spiking script result](../images/brainpan10.png  "Access violation in immunity debugger")

``Access violation`` means that we have overwritten the ``EIP``, ``EBP`` and ``ESP`` parts of the memory and can perform any buffer overflow from now on.

___

New to [Fuzzing using Python](https://github.com/cris-m/buffer_overflow_exploit_development/blob/main/assets/documentations/fuzzing.md)
