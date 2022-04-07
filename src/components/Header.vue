<template>
    <header class="header">
        <div class="container">
            <div class="row">
                <div class="col-12">
                    <div class="header__info">
                        <div class="header__name">{{ name }}</div>
                        <div class="header__img">
                            <img :src="avatar" alt="">
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </header>
</template>
<script>
export default {
    props: {
        name: {type: String, required: true},
        avatar: {type: String, required: true}
    }
}
</script>