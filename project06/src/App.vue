<template lang="html">
    <div id="app">
        <WishHeader></WishHeader>
        <!-- 앞에 addWish는 이벤트(하위에서 함수 호출하는 이름), 뒤의 addWish는 App.vue에 정의한 함수 -->
        <!-- v-on은 @로 대체할수 있다. -->
        <WishInput v-on:addWish="addWish"></WishInput>
        <WishList v-bind:propsData="wishItems" @removeWish="removeWish" @editWish="editWish"></WishList>
        <WishFooter v-on:removeAllofChild="removeAllofParents"></WishFooter>
    </div>
</template>
<script>
    import WishHeader from './components/WishHeader.vue'
    import WishInput from './components/WishInput.vue'
    import WishList from './components/WishList.vue'
    import WishFooter from './components/WishFooter.vue'
    export default{
        data(){
            return{
                wishItems: []
            }
        },
        created(){
            if(localStorage.getItem("vue-wish")){
                this.wishItems = JSON.parse(localStorage.getItem("vue-wish"))
                //for(var i = 0; i <localStorage.length; i++){
                //    this.wishItems.push(localStorage.key(i));
                //}
                this.wishItems.sort(function(a,b){
                    return a.key < b.key ? -1 : a.key > b.key ? 1 : 0
                    /*
                    var arr = [{key: 3}, {key: 1}, {key: 2}];
                    arr.sort(function(a,b){
                        return a.key < b.key ? -1 : a.key > b.key ? 1 : 0;
                    });
                    console.log(arr);
                    */
                    // [{key: 1}, {key: 2}, {key: 3}]
                })
            }
        },
        methods:{
            addWish(key, value, date){
                this.wishItems.push({
                    key,
                    value,
                    createdDate: date,
                    modifiedDate: date
                })
                localStorage.setItem("vue-wish",JSON.stringify(this.wishItems))
                /*
                let car = {
                    no: "11가 1111",
                    name: "소나타",
                    maker: "현대",
                    cc: 2000,
                    year: 2021
                };,
                //자바스크립트 객체를 JSON 데이터로 변환
                var json = JSON.stringify(car);
                console.log(json);

                //console 출력 결과 : {"no":"11가 1111","name":"소나타","maker":"현대","cc":2000,"year": 2021}
                */
            },
            removeAllofParents(){
                this.wishItems = [];
                localStorage.setItem("vue-wish",JSON.stringify(this.wishItems))
            },
            removeWish(keyOfWishItem, index){
                this.wishItems.splice(index,1);
                localStorage.setItem("vue-wish",JSON.stringify(this.wishItems));
            },
            editWish(keyOfWishItem, index, editText, modifiedDate){
                //console.log(keyOfWishItem, index, editText, modifiedDate)
                const item = this.wishItems[index]
                this.wishItems.splice(index,1,{
                    key: keyOfWishItem,
                    value: editText,
                    createdDate: item.createdDate,
                    modifiedDate: modifiedDate
                })
                localStorage.setItem("vue-wish",JSON.stringify(this.wishItems))
            }
        },
        components:{
            'WishHeader': WishHeader,
            'WishInput': WishInput,
            'WishList': WishList,
            'WishFooter': WishFooter
        }
    }
</script>
<style lang="css">
    body{text-align: center; background-color: #f6f6f8;}
    input{border-style: groove; width: 200px;}
    button{border-style: groove;}
    .shadow{box-shadow: 5px 10px 10px rgba(0,0,0,0.03);}
</style>
