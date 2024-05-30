# -Retrieve-user-data
 Retrieve  user data from Firebase
public class MainActivity extends AppCompatActivity {
    private ListView listView;
    private DatabaseReference databaseReference;
    private List<String> dataList;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        listView = findViewById(R.id.listView);
        dataList = new ArrayList<>();

        // Initialize Firebase
        FirebaseDatabase firebaseDatabase = FirebaseDatabase.getInstance();
        databaseReference = firebaseDatabase.getReference("Users");

        // Retrieve data from Firebase
        databaseReference.addValueEventListener(new ValueEventListener() {
            @Override
            public void onDataChange(@NonNull DataSnapshot dataSnapshot) {
                // Clear the previous data
                dataList.clear();

                // Iterate through all data
                for (DataSnapshot snapshot : dataSnapshot.getChildren()) {
                    // Get the value of each child node and add it to the list
                    String value = snapshot.getValue().toString();
                    dataList.add(value);
                }

                // Update the ListView with the new data
                ArrayAdapter<String> adapter = new ArrayAdapter<>(MainActivity.this, android.R.layout.simple_list_item_1, dataList);
                listView.setAdapter(adapter);
            }

            @Override
            public void onCancelled(@NonNull DatabaseError databaseError) {
                // Handle errors
            }
        });
    }
}
