FROM python:alpine

# Uygulama dizinini oluştur
RUN mkdir -p /app && chown -R 1001:0 /app

WORKDIR /app

# Gereksinimleri yükle
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Uygulama dosyalarını kopyala
COPY . .

# Kullanıcıyı değiştir (OpenShift'in rastgele UID'si ile çalışmasına izin verir)
USER 1001

# Portu aç
EXPOSE 80

# Flask uygulamasını başlat
CMD ["python", "./app.py"]
