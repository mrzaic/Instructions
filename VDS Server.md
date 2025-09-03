<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>VDS Server</title>
  <link rel="stylesheet" href="https://stackedit.io/style.css" />
</head>

<body class="stackedit">
  <div class="stackedit__left">
    <div class="stackedit__toc">
      
<ul>
<li><a href="#покупка-сервера">Покупка сервера</a></li>
<li><a href="#настройка-с-нуля">Настройка с нуля</a>
<ul>
<li><a href="#первичное-обновление-ос">Первичное обновление ОС</a></li>
<li><a href="#установка-docker">Установка Docker</a></li>
<li><a href="#установка-portainer">Установка Portainer</a></li>
<li><a href="#установите-wireguard">Установите WireGuard</a></li>
</ul>
</li>
</ul>

    </div>
  </div>
  <div class="stackedit__right">
    <div class="stackedit__html">
      <h1 id="покупка-сервера">Покупка сервера</h1>
<h1 id="настройка-с-нуля">Настройка с нуля</h1>
<h2 id="первичное-обновление-ос">Первичное обновление ОС</h2>
<p>Установите все обновления Ubuntu. Выполните команды:</p>
<pre><code>sudo apt update &amp;&amp; sudo apt upgrade -y
</code></pre>
<p>Дождитесь окончания обновлений и перезагрузитесь:</p>
<pre><code>reboot
</code></pre>
<p>После входа должны увидеть строку:</p>
<blockquote>
<p>0 updates can be applied immediately.</p>
</blockquote>
<h2 id="установка-docker">Установка Docker</h2>
<p>Установите пакеты, которые необходимы для работы пакетного менеджера apt по протоколу HTTPS:</p>
<pre><code>sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
</code></pre>
<p>Для установки используем официальную инструкцию:</p>
<p><a href="https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository">https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository</a></p>
<p>Добавьте GPG-ключ и сам репозитория Docker:</p>
<pre><code># Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release &amp;&amp; echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list &gt; /dev/null
sudo apt-get update
</code></pre>
<p>Установите Docker</p>
<pre><code>sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
</code></pre>
<p>Что сделали команды выше:</p>
<ol>
<li>Добавили официальный GPG ключ Docker (/etc/apt/keyrings/docker.asc) для проверки пакетов.</li>
<li>Добавили репозиторий Docker в apt.</li>
<li>Обновили индексы пакетов (apt-get update).</li>
<li>Установили пакеты:
<ul>
<li><code>docker-ce</code> — сам движок Docker</li>
<li><code>docker-ce-cli</code> — командная утилита</li>
<li><code>containerd.io</code> — движок контейнеризации</li>
<li><code>docker-buildx-plugin</code> — расширенные возможности сборки контейнеров</li>
<li><code>docker-compose-plugin</code> — встроенный Docker Compose</li>
</ul>
</li>
</ol>
<p>Проверяем, что Docker установился:</p>
<pre><code>docker --version
docker compose version
sudo systemctl status docker
</code></pre>
<p>В ответ видим примерно следующее:</p>
<blockquote>
<p>Docker version 28.3.3, build 980b856<br>
Docker Compose version v2.39.1</p>
</blockquote>
<h2 id="установка-portainer">Установка Portainer</h2>
<p>Используем и сверяемся с официальной инструкцией:</p>
<p><a href="https://docs.portainer.io/start/install-ce/server/docker/linux">https://docs.portainer.io/start/install-ce/server/docker/linux</a></p>
<p>Создаём Docker volume для данных Portainer:</p>
<pre><code>docker volume create portainer_data
</code></pre>
<p>Ставим и запускаем Portainer:</p>
<pre><code>docker run -d \
-p 8000:8000 \
-p 9443:9443 \
--name portainer \
--restart=always \
-v /var/run/docker.sock:/var/run/docker.sock \
-v portainer_data:/data \
portainer/portainer-ce:lts
</code></pre>
<p>Проверяем:</p>
<pre><code>docker ps
</code></pre>
<p>Должны увидеть запущенный контейнер с Портенером :)</p>
<p>Открываем в браузере адрес:</p>
<p><code>https://&lt;IP_твоего_VDS&gt;:9443</code></p>
<p>Видим первое окно Portainer и задаём пароль для пользователя <code>admin</code>.</p>
<p>Если возникнет сообщение о необходимости перезапустить Portainer после установки пароля:</p>
<pre><code>docker restart portainer
</code></pre>
<p>После входа в Portainer выберите:</p>
<p><code>Get Started</code></p>
<p><code>Proceed using the local environment which Portainer is running in</code></p>
<h2 id="установите-wireguard">Установите WireGuard</h2>
<p>Смотри официальную инструкцию:</p>
<p>sudo apt update<br>
sudo apt install wireguard</p>

    </div>
  </div>
</body>

</html>
