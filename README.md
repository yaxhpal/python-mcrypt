python-mcrypt
=============

The now deprecated python-mcrypt for python 2.4+.

The mcrypt library provides an easy to use interface for several algorithms and modes of cryptography. The algorithms SERPENT, RIJNDAEL, 3DES, GOST, SAFER+, CAST-256, RC2, XTEA, 3WAY, TWOFISH, BLOWFISH, ARCFOUR, and WAKE are some of the supported by the library.

This module exports functionality provided by the mcrypt library to python programs.


Installation
------------

python setup.py build

sudo python setup.py install


Classes
-------

Instances of this class, or subclasses of it, may be used to encyrpt and decrypt strings and files. This is a newstyle class, and this may be subclassed by python classes.

MCRYPT(algorithm, mode [, algorithm_dir, mode_dir])


Functions
---------

These functions are used to extract information about algorithms and modes, without creating an MCRYPT instance. Instances of MCRYPT may access most of this data about the selected algorithm and mode trough its methods.

set_algorithm_dir(algorithm_dir)
set_mode_dir(mode_dir)
list_algorithms([algorithm_dir])
list_modes([mode_dir])
is_block_algorithm(algorithm [, algorithm_dir])
is_block_mode(mode [, mode_dir])
is_block_algorithm_mode(mode [, mode_dir])
get_block_size(algorithm [, algorithm_dir])
get_key_size(algorithm [, algorithm_dir])
get_key_sizes(algorithm [, algorithm_dir])


Constants
---------

MCRYPT_*


MCRYPT Instance

This is the main class. Its instances offer encryption and decryption functionality. MCRYPT is implemented as a newstyle class. It means you may subclass it in your python programs and extend its functionality. Don't forget to call its __init__ method if you do this.


Constructor
-----------

MCRYPT(algorithm, mode [, algorithm_dir, mode_dir])

The first parameter is a string containing the name of a known cryptography algorithm. The second parameter is a string containing the name of a known cryptography mode. The third and fourth parameters are used to select the directory where the mcrypt library will look for the respective modules. You usually won't have to set these, and if you have to, you may want to use the set_algorithm_dir() and set_mode_dir() functions provided in the mcrypt module, so you won't have to set these everytime you instantiate an MCRYPT object.


Methods
-------

You may check the inline documentations of each of this methods for more information about them.

init(key [, iv]) -> None

This method initializes all buffers for the MCRYPT instance. The maximum length of key should be the one obtained by calling MCRYPT.get_key_size(). You must also check MCRYPT.get_key_sizes(), which will return an empty list, if every value smaller than the maximum length is legal, or a list of legal key lengths. Note that the key length is specified in bytes not bits. The iv parameter, if given, must have the size obtained with MCRYPT.get_iv_size().  It needs to be random and unique (but not secret). The same iv must be used for encryption/decryption. Even if this parameter is not obligatory, its use is recommended.  You must call this function before starting to encrypt or decrypt something. If you want to use the same key and iv to encrypt and/or decrypt data repeatedly, you may use MCRYPT.reinit() as a faster alternative.

reinit() -> None

If you have encrypted or decrypted something and want to encrypt or decrypt something else with the same key and iv, you may use this method as a faster alternative to using init() with the same parameters as before. Note that you can't call this method in an uninitialized instance.

deinit() -> None

This method clear all buffers and deinitialize the instance. After calling it you won't be able to use the reinit() method, and will have to call init() if you want to use one of the encrypt or decrypt methods.

Calling this method is optional.

encrypt(data [, fixlength=0]) -> string

This is the main decryption function. If fixlength is 1 than a trick will be used to keep the original data size when decrypting. This trick consists of using the last byte to keep the number of used bytes in the last block (when all bytes are used, an empty block has to be added to support this). Note that for the trick to work, you must enable it in encryption as well (to understand the trick, you may want to enable it for encrypt, and not for decrypt).

decrypt(data [, fixlength=0]) -> string

This is the main encryption function. If using a block algorithm, and data size is not a multiple of the block size, data will be padded with zeros before the encryption. If fixlength is 1 than a trick will be used to keep the original data size when decrypting. This trick consists of using the last byte to keep the number of used bytes in the last block (when all bytes are used, an empty block has to be added to support this). Note that for the trick to work, you must enable it in decryption as well (to understand the trick, you may want to enable it for encrypt, and not for decrypt).

encrypt_file(filein, fileout, [, fixlength=0, bufferblocks=1024]) -> data

You may use this function to encrypt files. If using a block algorithm, and data size is not a multiple of the block size, data will be padded with zeros before the encryption. If fixlength is 1 than a trick will be used to keep the original data size when decrypting. This trick consists of using the last byte to keep the number of used bytes in the last block (when all bytes are used, an empty block has to be added to support this). Note that for the trick to work, you must enable it in decryption as well (to understand the trick, you may want to enable it for encrypt, and not for decrypt). The bufferblocks parameter allows you to set the buffer size that will be used to transfer data between the files (buffer_size = bufferblocks*block_size).

decrypt_file(filein, fileout, [, fixlength=0, bufferblocks=1024]) -> data

You may use this function to decrypt files. If fixlength is 1 than a trick will be used to keep the original data size when decrypting. This trick consists of using the last byte to keep the number of used bytes in the last block (when all bytes are used, an empty block has to be added to support this). Note that for the trick to work, you must enable it in encryption as well (to understand the trick, you may want to enable it for encrypt, and not for decrypt). The bufferblocks parameter allows you to set the buffer size that will be used to transfer data between the files (buffer_size = bufferblocks*block_size).

get_block_size() -> int

Returns the block size of the algorithm used by the instance.

get_key_size() -> int

This method returns the maximum key size supported by the algorithm used in the MCRYPT instance. To know the acceptable key sizes, you must check the get_key_sizes() method.


get_key_sizes() -> list

This method returns a list of key sizes supported by the algorithm used in the MCRYPT instance. If this list is empty, any length between 1 and the maximum key size (returned by the get_key_size() function) may be used.

get_iv_size() -> int

This method returns the iv size supported by the algorithm used in the MCRYPT instance.

is_block_algorithm() -> bool

Returns 1 if the algorithm is a block algorithm or 0 if it is a stream algorithm.

is_block_mode() -> bool

is_block_algorithm_mode() -> bool

Returns 1 if the mode is for use with block algorithms, otherwise it returns 0. (eg. 0 for stream, and 1 for cbc, cfb, ofb).


Attributes
----------

algorithm   - Selected algorithm.
mode        - Selected mode.

