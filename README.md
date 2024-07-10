# first-one

# Skopiowanie nowego certyfikatu do odpowiedniego folderu (jeśli jest w innej lokalizacji)
cp /path/to/sub-ca-cert.pem /home/dsl/.local/lib/python3.8/site-packages/certifi/

# Otwieranie pliku cacert.pem w edytorze nano
nano /home/dsl/.local/lib/python3.8/site-packages/certifi/cacert.pem

# Skopiowanie zawartości sub-ca-cert.pem na koniec pliku cacert.pem
cat /path/to/sub-ca-cert.pem >> /home/dsl/.local/lib/python3.8/site-packages/certifi/cacert.pem

# Zapisanie zmian i zamknięcie edytora nano (Ctrl+X, Y, Enter)




pip show certifi

cd /home/dsl/.local/lib/python3.8/site-packages/certifi

nano cacert.pem

