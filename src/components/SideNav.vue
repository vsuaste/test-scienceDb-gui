<template>
  <div class="sidenav">
    <a href="/home"> HOME </a>
        <a href='/books'> Books   </a>
        <a href='/dogs'> Dogs   </a>
        <a href='/people'> People   </a>
        <a href='/projects'> Projects   </a>
        <a href='/publishers'> Publishers   </a>
        <a href='/researchers'> Researchers   </a>
        <a href='/species'> Species   </a>
        <a href='/individuals'> individuals   </a>
        <a href='/transcript_counts'> transcript_counts   </a>
      </div>
</template>

<script>

export default {
  name: 'side-nav'
}
</script>

<style>
.sidenav {
    text-align: left;
    height: 100%;
    width: 280px;
    position: fixed;
    z-index: 1;
    top: 0;
    left: 0;
    background-color: #111;
    overflow-x: hidden;
    padding-top: 20px;
}
.sidenav a {
    padding: 6px 6px 6px 32px;
    text-decoration: none;
    color: #818181;
    display: block;
}
.sidenav a:hover {
    color: #f1f1f1;
}

@media screen and (max-height: 450px) {
  .sidenav {padding-top: 15px;}
  .sidenav a {font-size: 18px;}
}
</style>
