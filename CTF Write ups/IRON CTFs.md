Forensics
Title: Random pixels
I was given a photo which was scrambled but also encrypted ,but lucky enough there was the encrypting script was provided.
Encrypted picture
![[encrypted.png]]

source code
`import random, time, numpy`
`from PIL import Image`
`from secret import FLAG`

`def randomize(img, seed):`
	`random.seed(seed)`
	`new_y = list(range(img.shape[0]))`
	`new_x = list(range(img.shape[1]))`
	`random.shuffle(new_y)`
	`random.shuffle(new_x)`

	`new = numpy.empty_like(img)`
	`for i, y in enumerate(new_y):`
		`for j, x in enumerate(new_x):`
			`new[i][j] = img[y][x]`
	`return numpy.array(new)`


`if __name__ == "__main__":`
	`with Image.open(FLAG) as f:`
		`img = numpy.array(f)`
		`out = randomize(img, int(time.time()))`
		`image = Image.fromarray(out)`
		`image.save("encrypted.png")`

I analysed the code but could not solve ,so i uploaded both files to chat gpt 4 it gave me an idea and wrote the decryption script which i had to tweak to make it work perfectly.

`import random`
`import numpy as np`
`from PIL import Image`
`import time`

--Function to reverse the pixel randomization
`def derandomize(img, seed):`
    `random.seed(seed)`
    `new_y = list(range(img.shape[0]))`
    `new_x = list(range(img.shape[1]))`
    `random.shuffle(new_y)`
    `random.shuffle(new_x)`
    
    `reverse_y = np.argsort(new_y)`
    `reverse_x = np.argsort(new_x)`

    `new = np.empty_like(img)`
    `for i, y in enumerate(reverse_y):`
        `for j, x in enumerate(reverse_x):`
            `new[i][j] = img[y][x]`
    `return np.array(new)`

--Load the encrypted image
`encrypted_image = Image.open("encrypted.png")`
`img = np.array(encrypted_image)`

--Use the  File Modification Date/Time  time as the seed ,which can be extracted with exiftool
`inode_change_seed = int(time.mktime(time.strptime("2024:10:02 23:02:40", "%Y:%m:%d %H:%M:%S")))`

--Reverse the encryption
`reversed_image_array = derandomize(img, inode_change_seed)`
`reversed_image = Image.fromarray(reversed_image_array)`

--Save the decrypted image
`reversed_image.save("decrypted_image.png")`

Decrypted picture
![[decrypted_image.png]]

scanned it and got the flag ,it was worth 500 points