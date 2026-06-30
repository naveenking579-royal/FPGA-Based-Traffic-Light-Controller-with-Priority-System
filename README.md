# FPGA-Based-Traffic-Light-Controller-with-Priority-System


entity Traffic_Light_Controller is
    Port (
        clk       : in  STD_LOGIC;
        reset     : in  STD_LOGIC;
        emergency : in  STD_LOGIC;
        roadA     : out STD_LOGIC_VECTOR (2 downto 0);
        roadB     : out STD_LOGIC_VECTOR (2 downto 0)
    );
end Traffic_Light_Controller;

architecture Behavioral of Traffic_Light_Controller is

    type state_type is (S0, S1, S2, S3);
    signal state : state_type := S0;

    signal count : integer range 0 to 9 := 0;

begin

process(clk, reset)

begin

    if reset='1' then
        state <= S0;
        count <= 0;

    elsif rising_edge(clk) then

        if emergency='1' then
            state <= S2;

        else

            if count=9 then
                count <= 0;

                case state is

                    when S0 =>
                        state <= S1;

                    when S1 =>
                        state <= S2;

                    when S2 =>
                        state <= S3;

                    when S3 =>
                        state <= S0;

                end case;

            else
                count <= count + 1;
            end if;

        end if;

    end if;

end process;


process(state)

begin

case state is

when S0 =>
    roadA <= "001";
    roadB <= "100";

when S1 =>
    roadA <= "010";
    roadB <= "100";

when S2 =>
    roadA <= "100";
    roadB <= "001";

when S3 =>
    roadA <= "100";
    roadB <= "010";

end case;

end process;

end Behavioral;