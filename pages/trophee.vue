<template>
  <Header />
  <div class="trophee-container">
    <div class="filtre-container">
      <ul>
        <li><a href="#" class="actual-link">Classement mondial</a></li>
        <li><a href="/friends">Mes amis</a></li>
      </ul>
    </div>
    <table style="border-collapse: collapse; width: 100%">
      <thead>
        <tr>
          <th
            style="
              text-align: center;
              padding: 8px;
              border-bottom: 2px solid #000;
              width: 33.33%;
            "
          >
            Rang
          </th>
          <th
            style="
              text-align: center;
              padding: 8px;
              border-bottom: 2px solid #000;
              width: 33.33%;
            "
          >
            User
          </th>
          <th
            style="
              text-align: center;
              padding: 8px;
              border-bottom: 2px solid #000;
              width: 33.33%;
            "
          >
            Feuille
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(user, index) in sortedUsers" :key="user.id">
          <td style="padding: 8px; text-align: center" v-if="index + 1 == 1">
            <img
              class="medals"
              src="/public/imgs/medailles/or.png"
              alt="medaille d'or"
            />
          </td>
          <td
            style="padding: 8px; text-align: center"
            v-else-if="index + 1 == 2"
          >
            <img
              class="medals"
              src="/public/imgs/medailles/argent.png"
              alt="medaille d'argent"
            />
          </td>
          <td
            style="padding: 8px; text-align: center"
            v-else-if="index + 1 == 3"
          >
            <img
              class="medals"
              src="/public/imgs/medailles/bronze.png"
              alt="medaille de bronze"
            />
          </td>
          <td style="padding: 8px; text-align: center" v-else>
            {{ index + 1 }}
          </td>
          <td style="padding: 8px; text-align: center">{{ user.username }}</td>
          <td style="padding: 8px; text-align: center">{{ user.points }}</td>
        </tr>
      </tbody>
    </table>
  </div>
  <Footer />
</template>

<script setup>
definePageMeta({
  middleware: "auth",
});
useHead({
  title: "Trophée - Ecosphere",
  meta: [{ name: "description", content: "This is the trophee's page" }],
});

const { data: userList } = await useFetch("/api/get-every-user");

const sortedUsers = computed(() => {
  return [...userList.value].sort((a, b) => b.points - a.points).slice(0, 15);
});
</script>

<style scoped>
.trophee-container {
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: center;
  flex: 1 1 0%;
  overflow: auto;

  .actual-link {
    font-weight: bold;
    text-decoration: underline;
  }

  hr {
    height: 1px;
    width: 100px;
  }

  .filtre-container {
    ul {
      display: flex;
      flex-direction: row;
      justify-content: center;
      align-items: center;
      gap: 50px;

      li {
        a {
          font-size: 0.8em;
        }

        a:hover {
          text-decoration: underline;
        }
      }
    }

    table {
      margin-left: 20px;
    }
  }
}

.footer-container {
  width: 100%;
}

.medals {
  width: 30px;
  display: block;
  margin: 0 auto;
}
</style>
