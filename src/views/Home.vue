<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/949438u">
        <img src="https://www.modern-notoriety.com/wp-content/uploads/2019/03/hole.jpg" alt="naroto" class="featured-img" />
        <div class="detail">
          <h3>Naruto</h3>
          <p>Lorem ipsum, dolor sit amet consectetur adipisicing elit. Repudiandae, adipisci enim officiis consequuntur quia nisi veritatis illum? Quaerat, quibusdam ipsa! Nemo veniam distinctio quo dignissimos sunt! Modi deleniti possimus inventore?Lorem ipsum, dolor sit amet consectetur adipisicing elit. Repudiandae, adipisci enim officiis consequuntur quia nisi veritatis illum? Quaerat, quibusdam ipsa! Nemo veniam distinctio quo dignissimos sunt! Modi deleniti possimus inventore?</p>

        </div>
      </router-link>
    </div>
    <form class="search-box" @submit.prevent="searchMovies()">
      <input type="text" placeholder="what are you looking for ? " v-model="search"/>
      <input type="submit" value="Search"/>
    </form>
    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbID">
        <router-link  :to="'/movie/'+movie.imdbID" class="movie-link">
          <div class="product-image">
            <img :src="movie.Poster" alt="Movie poster"/>
            <div class="type">{{movie.Type}}</div>
          </div>
          <div class="detail">
            <p class="year">{{movie.Year}}</p>
            <h3>{{movie.Title}}</h3>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import {ref} from 'vue';
import env from '../env';
export default {
  setup(){
    const search = ref('');
    const movies = ref([]);
    

    const searchMovies = ()=>{
      if(search.value != ""){
        fetch(`http://www.omdbapi.com/?i=tt3896198&apikey=${env.apiKey}&s=${search.value}`)
        .then(response =>response.json())
        .then(data=>{
          movies.value = data.Search;
          search.value ="";
          console.log(movies.value);
        })
      }
    }
    return {
      search,
      movies,
      searchMovies
    }
  }
}
</script>

<style lang="scss">
  .home{
    .feature-card{
      position: relative;

      .featured-img{
        display: block;
        width:100%;
        height: 300px;
        object-fit: cover;
        position: relative;
        z-index:0;
      }
      .detail{
        position: absolute;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0,0,0,.6);
        padding: 16px;
        z-index: 1;

        h3{
          color:#fff;
          margin-bottom: 16px;
        }
        p{
          color: #fff;
        }
      }
    }
    .search-box{
      display: flex;
      flex-direction: column;
      justify-content: center;
      padding: 16px;
      align-items: center;

      input{
        display: block;
        appearance: none;
        border: none;
        outline: none;
        background:none;

        &[type="text"]{
          width: 100%;
          color:#fff;
          background-color: #496583;
          font-size: 20px;
          padding: 10px 16px;
          border-radius: 8px;
          margin-bottom: 15px;
          transition: .4s;

          &::placeholder{
            color:#f3f3f3;
          }

          &:focus{
            box-shadow: 0px 3px 6px rgba(0,0,0,.2);
          }
        }
        &[type="submit"]{
            width: 100%;
            max-width: 300px;
            background-color: #42b883;
            padding: 16px;
            border-radius: 8px;
            color: #fff;
            text-transform: uppercase;
            font-size: 20px;
            transition: .4s;

            &:active{
              background-color: #3b8070;
            }
          }
      }
    }
    .movies-list{
      display: flex;
      flex-wrap: wrap;
      margin: 0 8px;

      .movie{
        max-width: 50%;
        flex: 1 1 50%;
        padding: 16px 8px;

        .movie-link{
          display: flex;
          flex-direction: column;
          height: 100%;
        
          .product-image{
            position: relative;
            display: block;

            img{
              display: block;
              width:100%;
              height: 275px;
              object-fit: cover;
            }
            .type{
              position: absolute;
              padding: 8px 16px;
              background-color: #428843;
              color:#fff;
              bottom: 16px;
              left:0;
              text-transform: capitalize;
            }
          }
        .detail{
          background-color: #496583;
          padding: 16px 8px;
          flex: 1 1 100%;
          border-radius: 0 0 8px 8px;

          .year{
            color:#aaa;
            font-size: 16px;
          }
          h3{
            color:#fff;
            font-weight: bold;
            font-size: 19px;
          }
        }
        }

      }
    }
  }
</style>