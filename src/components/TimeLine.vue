<style>
.time-line-zxh {
    position: relative;
    padding-left: 30px;
    font-size: 14px;
    color: #333;
    /* background: #fff; */
    overflow: hidden;
}
.time-line-zxh:before {
    content: "";
    position: absolute;
    left: 10px;
    width: 1px;
    height: 100%;
    background: rgb(24,144,255);
}
.time-line-item-zxh {
    position: relative;
    margin: 7px 0;
}
.time-line-item-zxh:before {
    content: "";
    position: absolute;
    left: -25px;
    top: 3px;
    width: 12px;
    height: 12px;
    border-radius: 100%;
    background: #f2f2f2;
    border: 2px solid rgb(24,144,255)
}
</style>
<template>
    <div class="time-line-zxh">
        <div class="time-line-item-zxh">
            <slot name="lists"></slot>
        </div>
    </div>
</template>
<script>
export default {

};
</script>
