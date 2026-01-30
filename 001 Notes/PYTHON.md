PATH env variable is updated to point to the virtual environment's bin directory when that virtual env is activated.

# REQUESTS
response = requests.get(url)
    İndirme varsa mesela dosya direkt ram'e alınır.
    Satır bitmeden dosya inmiş olur.
response = requests.get(url, stream=True)
    Stream verince sadece headerler alınır.
    response.iter_content dediğinde parça parça veri alırsın.