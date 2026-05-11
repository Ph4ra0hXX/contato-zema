<script setup>
const logoUrl = "/logo.svg";

const placeholderPhoto = "/vendors/placeholder.svg";

const categories = [
  {
    id: "eletro",
    label: "Eletro & Eletrônicos",
    hint: "TVs, celulares, informática, acessórios.",
    areas: [
      {
        id: "eletro-thaiz",
        name: "Thaiz",
        whatsapp: "556299406750",
        phone: "62 9940-6750",
        photo: "/vendors/thaiz.jpg",
      },
      {
        id: "eletro-layla",
        name: "Layla",
        whatsapp: "553498568018",
        phone: "34 98568018",
        photo: "/vendors/layla.jpg",
      },
    ],
  },
  {
    id: "moda",
    label: "Moda",
    hint: "Vestuário, calçados e acessórios.",
    areas: [
      {
        id: "moda-ana",
        name: "Ana",
        whatsapp: "553496871070",
        phone: "34 96871070",
        photo: "/vendors/ana.jpg",
      },
      {
        id: "moda-gielvanna",
        name: "Gielvanna",
        whatsapp: "556295081106",
        phone: "62 95081106",
        photo: "/vendors/gielvanna.jpg",
      },
    ],
  },
  {
    id: "emprestimo",
    label: "Empréstimo",
    hint: "Crédito e empréstimos.",
    areas: [
      {
        id: "emprestimo-whatsapp",
        name: "Empréstimo",
        whatsapp: "553499447031",
        phone: "34 9944-7031",
        photo: "/vendors/placeholder.svg",
      },
    ],
  },
];

function avatarSrc(area) {
  return area.photo ?? placeholderPhoto;
}

function onAvatarError(event) {
  const img = event?.target;
  if (img && img.tagName === "IMG") {
    img.src = placeholderPhoto;
  }
}

function buildBody(categoryLabel, areaName) {
  const lines = [
    `Olá! Gostaria de falar com a área "${areaName}".`,
    "",
    `Categoria: ${categoryLabel}`,
    "",
    "Podem me orientar sobre o melhor atendimento?",
  ];
  return lines.join("\n");
}

function whatsappHref(area) {
  const text = encodeURIComponent(buildBody(area.categoryLabel, area.name));
  return `https://wa.me/${area.whatsapp}?text=${text}`;
}
</script>

<template>
  <div class="page">
    <div class="logoBar" aria-label="Logo">
      <img class="logo" :src="logoUrl" alt="Logo" />
    </div>

    <main class="content">
      <section
        v-for="cat in categories"
        :key="cat.id"
        class="panel"
        :aria-label="`Categoria ${cat.label}`"
      >
        <div class="panelTitle">
          <h2>{{ cat.label }}</h2>
          <p v-if="cat.hint">{{ cat.hint }}</p>
        </div>

        <div class="areas" v-if="cat.areas.length">
          <article v-for="area in cat.areas" :key="area.id" class="area">
            <div class="areaHead">
              <div class="areaIdentity">
                <img
                  class="avatar"
                  :src="avatarSrc(area)"
                  :alt="`Foto da ${area.name}`"
                  loading="lazy"
                  @error="onAvatarError"
                />
                <h3>{{ area.name }}</h3>
              </div>
              <span v-if="area.hours" class="hours">{{ area.hours }}</span>
            </div>

            <div class="actions">
              <a
                class="btn"
                :href="whatsappHref({ ...area, categoryLabel: cat.label })"
                target="_blank"
                rel="noopener"
              >
                Abrir WhatsApp
              </a>
            </div>
          </article>
        </div>
      </section>
    </main>
  </div>
</template>

<style scoped>
.page {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.logoBar {
  display: flex;
  justify-content: center;
  padding: 0.5rem 0;
}

.logo {
  width: 250px;
  height: auto;
  display: block;
}

.content {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1.25rem;
}

.panel {
  border: 1px solid var(--color-border);
  border-radius: 16px;
  background: var(--color-background);
  padding: 1.25rem;
  color: var(--color-text);
}

.panelTitle {
  padding-left: 0.85rem;
  border-left: 4px solid var(--color-primary);
}

.panelTitle h2 {
  font-size: 1.25rem;
  font-weight: 750;
  line-height: 1.15;
  color: var(--color-heading);
}

.panelTitle p {
  margin-top: 0.25rem;
  opacity: 0.85;
}

.field {
  display: grid;
  gap: 0.35rem;
  margin-bottom: 1rem;
}

.field span {
  font-weight: 600;
  color: var(--color-heading);
  font-size: 0.95rem;
}

.field small {
  opacity: 0.8;
}

select,
input,
textarea {
  width: 100%;
  border: 1px solid var(--color-border);
  border-radius: 12px;
  padding: 0.75rem 0.85rem;
  background: var(--color-background);
  color: var(--color-text);
  outline: none;
}

select:focus,
input:focus,
textarea:focus {
  border-color: var(--color-border-hover);
  box-shadow: 0 0 0 3px rgba(242, 226, 5, 0.25);
}

.areas {
  margin-top: 1rem;
  display: grid;
  grid-template-columns: 1fr;
  gap: 0.9rem;
}

.area {
  position: relative;
  overflow: hidden;
  border: 1px solid var(--color-border);
  border-radius: 18px;
  padding: 1.05rem 1rem 1rem 1.15rem;
  background: var(--color-background-soft);
  color: var(--color-text);
}

.area::before {
  content: "";
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  width: 6px;
  background: var(--color-primary);
}

.areaHead {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 0.75rem;
}

.areaIdentity {
  display: flex;
  align-items: center;
  gap: 0.85rem;
  min-width: 0;
}

.avatar {
  width: 76px;
  height: 76px;
  border-radius: 999px;
  aspect-ratio: 1 / 1;
  object-fit: cover;
  object-position: center 20%;
  border: 1px solid var(--color-border);
  outline: 2px solid var(--color-primary);
  outline-offset: 1px;
  background: var(--color-background-mute);
  flex: 0 0 auto;
}

.areaHead h3 {
  font-size: 1rem;
  font-weight: 650;
  color: var(--color-heading);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.hours {
  font-size: 0.85rem;
  opacity: 0.8;
}

.actions {
  margin-top: 1rem;
  display: flex;
  flex-wrap: wrap;
  gap: 0.6rem;
}

.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  padding: 0.7rem 0.9rem;
  border-radius: 14px;
  border: 1px solid var(--color-border);
  background: var(--color-primary);
  color: var(--color-primary-text);
  text-decoration: none;
  font-weight: 700;
  letter-spacing: 0.01em;
  cursor: pointer;
  user-select: none;
  -webkit-tap-highlight-color: transparent;
  transform: translateY(0);
  box-shadow:
    0 6px 0 0 var(--vt-c-divider-dark-2),
    0 14px 18px -14px var(--vt-c-divider-dark-1);
  transition:
    transform 140ms ease,
    box-shadow 140ms ease,
    filter 140ms ease;
}

.btn:hover {
  filter: brightness(0.98);
  transform: translateY(-1px);
  box-shadow:
    0 7px 0 0 var(--vt-c-divider-dark-2),
    0 16px 20px -14px var(--vt-c-divider-dark-1);
}

.btn:active {
  transform: translateY(2px);
  box-shadow:
    0 3px 0 0 var(--vt-c-divider-dark-2),
    0 10px 14px -14px var(--vt-c-divider-dark-1);
}

.btn:focus-visible {
  outline: 2px solid var(--color-primary);
  outline-offset: 3px;
}

.btnGhost {
  background: transparent;
  color: var(--color-heading);
}

.btnGhost:hover {
  background: rgba(242, 226, 5, 0.2);
}
</style>
