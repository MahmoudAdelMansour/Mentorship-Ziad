# FileUploader Class Algorithm

## 1. Initialize Class

- Create a class named `FileUploader`.
- Declare private properties `$request` and `$uploads`.
- Create a constructor that accepts a `Request` object and initializes `$request`.

## 2. Create `upload` Method

- Create a public method named `upload` that accepts an associative array of files and directories.
- Iterate over the array:
  - If the value is an array, iterate over its elements and call the `uploadFile` method for each file and directory.
  - If the value is not an array, call the `uploadFile` method for the file and directory.

## 3. Create `uploadFile` Method

- Create a private method named `uploadFile` that accepts a file name and a directory.
- Instantiate a `Mediaable` object with the provided `Request`.
- Set the directory using the `moveToDir` method.
- Use the `getMediaFromRequestByName` method to upload the file.
- Store the file name and path in the `$uploads` array.

## 4. Return Uploads

- After processing all files, return the `$uploads` array containing file names and paths.

<pre>

```
#Mediable Code 
class Mediaable
{

    /**
     * @var Request
     */
    private Request $request;
    /**
     * @var string
     */
    private string $move_to = 'uploads';
    /**
     * @var string
     */
    private string $path = '';


    /**
     * @param Request $request
     */
    public function __construct(Request $request)
    {
        $this->request = $request;
    }

    /**
     * @param string $dir
     * @return $this
     */
    public function moveToDir(string $dir): static
    {
        $this->move_to = $dir;
        return $this;
    }

    /**
     * @param string $name
     * @return Mediaable
     */
    public function getMediaFromRequestByName(string $name): Mediaable
    {
        if ($this->request->file($name)->store('public/' . $this->move_to)) {
            $this->path = $this->request->file($name)->store('public/' . $this->move_to);
        }
        return $this;
    }

    /**
     * @return string
     */
    public function getMediaNameAfterUpload(): string
    {
        return str_replace('public/', '', $this->path);
    }


}
```

