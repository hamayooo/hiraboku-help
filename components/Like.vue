<template>
  <section class="Like">
    <h2 class="Like_Heading">Was this article helpful?</h2>
    <div class="Like_Buttons">
      <label class="Like_Button">
        <input type="radio" name="like" />
        <span class="Like_Yes">
          <svg viewBox="0 0 17 15" xmlns="http://www.w3.org/2000/svg"><path d="M15.25 5.25h-4.7325l.7125-3.4275.0225-.24c0-.3075-.1275-.5925-.33-.795L10.1275 0l-4.935 4.9425C4.915 5.2125 4.75 5.5875 4.75 6v7.5c0 .825.675 1.5 1.5 1.5H13c.6225 0 1.155-.375 1.38-.915l2.265-5.2875c.0675-.1725.105-.3525.105-.5475v-1.5c0-.825-.675-1.5-1.5-1.5zm0 3L13 13.5H6.25V6l3.255-3.255-.8325 4.005H15.25v1.5zM.25 6h3v9h-3V6z" fill-rule="nonzero"/></svg>
        </span>
      </label>
      <label class="Like_Button">
        <input type="radio" name="like" />
        <span class="Like_No">
          <svg viewBox="0 0 17 15" xmlns="http://www.w3.org/2000/svg"><path d="M10.7 0H3.95c-.6225 0-1.155.375-1.38.915L.305 6.2025C.2375 6.375.2 6.555.2 6.75v1.5c0 .825.675 1.5 1.5 1.5h4.7325L5.72 13.1775l-.0225.24c0 .3075.1275.5925.33.795l.795.7875 4.935-4.9425c.2775-.27.4425-.645.4425-1.0575V1.5c0-.825-.675-1.5-1.5-1.5zm0 9l-3.255 3.255.8325-4.005H1.7v-1.5L3.95 1.5h6.75V9zm3-9h3v9h-3V0z" fill-rule="nonzero"/></svg>
        </span>
      </label>
    </div>
  </section>
</template>

<script>
export default {
  
}
</script>

<style scoped>
.Like {
  background: #f8f8f8;
  border-radius: 4px;
  padding: 24px;
  display: flex;
  align-items: center;
  margin: 40px 0;
}
.Like_Heading {
  margin: 0;
  padding: 0;
  font-size: 1.4rem;
}
.Like_Buttons {
  margin: 0 0 0 auto;
  display: flex;
  align-items: center;
}
.Like_Button {
  width: 40px;
  height: 40px;
  background: #fff;
  border-radius: 20px;
  box-shadow: 0 0 0 1px rgba(0,0,0,.06), 0 3px 10px -8px rgba(0,0,0,1);
  position: relative;
  cursor: pointer;
  margin: 0 0 0 16px;
  border: none;
}
.Like_Button:nth-child(1) {
  margin: 0;
}
.Like_Button > span > svg {
  position: absolute;
  width: 17px;
  height: 15px;
  left: 12px;
  fill: #CCC;
  transition: fill .2s;
}
.Like_Button:hover > span > svg {
  fill: #888;
}
.Like_Button > .Like_Yes > svg {
  top: 11px;
}
.Like_Button > .Like_No > svg {
  top: 14px;
}
.Like_Button > input {
  position: absolute;
  opacity: 0;
  -webkit-tap-highlight-color:rgba(0,0,0,0);
}
.Like_Button > input:checked + .Like_Yes > svg {
  fill: #006CDC;
  transition: none;
}
.Like_Button > input:checked + .Like_No > svg {
  fill: #DC0000;
  transition: none;
}
.Like_Button:active {
  transform: scale(.96);
}
.Like_Button:active > input + span::after,
.Like_Button > input:focus + span::after {
  content: "";
  position: absolute;
  left: 0;
  top: 0;
  width: 36px;
  height: 36px;
  border-radius: 20px;
  z-index: 0;
}
.Like_Button:active > input + .Like_Yes::after,
.Like_Button > input:focus + .Like_Yes::after {
  border: 2px solid #006CDC;
}
.Like_Button:active > input + .Like_No::after,
.Like_Button > input:focus + .Like_No::after {
  border: 2px solid #DC0000;
}
</style>