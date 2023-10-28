<template>
  <main>
    <div class="unload__wrapper">
      <div class="unload__left">
        <UnloadAbout>
          <h3 class="text-bold">Выгрузка</h3>
          <br />
          Выполняет работу:<br />
          - Собирает фотографии из заказов пользователей.<br />
          - Выгружает по папкам.
        </UnloadAbout>
        <div v-for="item in list" :key="item.id">
          <UnloadCard :status="item.status" @choose="chooseOne(item.id)">
            {{ item["task_date"] }}<br />
            Статус выдачи: <span class="text-bold">{{ item["status_text"] }}</span
            ><br />
            ID выгрузки: <span class="text-bold">{{ item.id }}</span
            ><br />
            {{ item.event }}<br />
            Размер выгрузки: <span class="text-bold">{{ item.size }}</span>
          </UnloadCard>
        </div>
      </div>
      <div class="unload__right">
        <p v-if="!cardInfo" class="notice unload__notice">
          Для того чтобы посмотреть информацию о <span class="text-bold">выгрузке</span> а также её скачать, нажмите на
          требуюмую выгрузку в столбце слева.
        </p>
        <div v-else class="unload__info">
          <div class="unload__info-top">
            <div class="unload__info-id">{{ cardInfo[0].id }}</div>
            <span class="unload__info-event text-bold">{{ cardInfo[0].event }}</span>
            <button class="unload__info-close" @click="clearInfo">x</button>
          </div>
          <div class="unload__info-body">
            <div class="unload__info-notice">
              <img src="/images/icons/idea.png" alt="notice" class="unload__info-img" />
              <span class="unload__info-text"
                >Если после клика загрузка не пошла, проверьте не блокирует ли браузер скачивание архива.</span
              >
            </div>
            <p class="unload__info-download">Ссылка для скачивания архива Выгрузки (.zip):</p>
            <div class="unload__info-bottom">
              <a :href="`https/://seenday.com/${parseNum(cardInfo[0].download_link)}`" class="unload__info-link">{{
                `https/://seenday.com/${parseNum(cardInfo[0].download_link)}`
              }}</a>
              <a
                href="#"
                class="unload__info-copy"
                @click.prevent="copyLink(`https/://seenday.com/${parseNum(cardInfo[0].download_link)}`)"
                >скопировать ссылку</a
              >
            </div>
            <button class="unload__info-exit" @click="clearInfo">Закрыть</button>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup>
const { data } = await useFetch("https://dev-cabinet.seenday.com/e.scripts?page=pages:unload&event=get");
const list = JSON.parse(data.value).response.data;

const cardInfo = ref();
async function chooseOne(id) {
  const { data } = await useFetch(
    `https://dev-cabinet.seenday.com/e.scripts?page=pages:unload&event=get&unload_id=${id}`
  );
  const res = JSON.parse(data.value).response.data;
  return (cardInfo.value = res);
}

function clearInfo() {
  cardInfo.value = "";
}

function parseNum(str) {
  return parseInt(str.match(/\d+/));
}

async function copyLink(str) {
  await navigator.clipboard.writeText(str);
}
</script>
