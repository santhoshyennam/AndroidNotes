package cubex.mahesh.railwayapi_trainstatus_oct7am

import android.support.v7.app.AppCompatActivity
import android.os.Bundle
import android.widget.ArrayAdapter
import cubex.mahesh.railwayapi_trainstatus_oct7am.beans.TrainStatusBean
import kotlinx.android.synthetic.main.activity_main.*
import retrofit2.Call
import retrofit2.Callback
import retrofit2.Response
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        getTrainStatus.setOnClickListener {
            var r = Retrofit.Builder().
                        addConverterFactory(GsonConverterFactory.create()).
                        baseUrl("https://api.railwayapi.com/").
                        build()
            var api:RailwayAPI = r.create(RailwayAPI::class.java)
            var call:Call<TrainStatusBean> = api.getTrainStatus(
                    et2.text.toString(),et1.text.toString())
            call.enqueue(object:Callback<TrainStatusBean>{
                override fun onFailure(call: Call<TrainStatusBean>, t: Throwable) {

                }

                override fun onResponse(call: Call<TrainStatusBean>, response: Response<TrainStatusBean>) {

                  var bean =  response.body()
                  var temp_list = mutableListOf<String>()
                  temp_list.add("Current Position : ${bean!!.position}")
                   var list =  bean.route
                   for(route in list!!)
                   {
                    temp_list.add("${route.station.name} \n " +
                      "Has Ar : ${route.hasArrived} \t Has Dpt: ${route.hasDeparted} \n" +
                            "Arr Time: ${route.scharr} \t Dept Time: ${route.schdep} \n"+
                                "Status : ${route.status}")
                   }
                    var adapter  = ArrayAdapter<String>(
                            this@MainActivity,R.layout.indiview,
                            temp_list)
                    lview.adapter = adapter



                }

            })
        }
    }
}
