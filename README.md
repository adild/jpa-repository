# jpa-repository

## Native query with aggregate functions - 

> Create Repository interface say BattingRepository which extends JpaRepository
```
BattingRepository extends JpaRepository<Balls, Integer>
```
> Note: 'Balls' is a entity class which matches with the table in database. <br>
> Create user defined method say getAllBatsmanAgainstTeam. 
```
BattingAgainstTeam getAllBatsmanAgainstTeam(@Param("s") String striker);
```
> Above method return type is an interface ie 'BattingAgainstTeam'. Due to this interface we are able to get the values we want instead of all the values present in 'Balls' entity. 'BattingAgainstTeam' looks like this - 
```
public interface BattingAgainstTeam {

  String getStriker();
  String getBattingTeam();
  String getBowlingTeam();

}
```
> Annotated the 'getAllBatsmanAgainstTeam' method with @Query. Looks like this -
```
@Query(value="select striker, ANY_VALUE(batting_team) as BattingTeam, ANY_VALUE(bowling_team) as BowlingTeam from balls where striker=:s group by match_id", nativeQuery = true)
BattingAgainstTeam getAllBatsmanAgainstTeam(@Param("s") String striker);
```
> Above @Query accepts 2 parameters ie 'value' is the sequel and 'nativeQuery=true' enables native query. <br>
> Carefully observe the query, 'as BattingTeam' is written coz to match the name present in 'BattingAgainstTeam' interface. <br>
> @Param("s") String striker is use to match the striker=:s in query. <br>
